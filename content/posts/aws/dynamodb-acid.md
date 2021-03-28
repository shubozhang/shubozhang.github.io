---
title: "NoSql也搞ACID：DynamoDB Transaction"
date: 2021-03-27
hero: hero.svg
menu:
    sidebar:
        name: dynamodb-acid
        identifier: dynamodb-acid
        parent: aws
        weight: 10
---

先扯扯为啥用DynamoDB，简单说就是公司没有或者没人愿意干DBA，SRE也不想干这活。。。感觉到了cloud后，DBA都华丽转身大数据了，不屑于在搞RDS。
还有些公司搞"you write it， you deploy it，and you maintain it"，一个组要负责自己的产品，从UI到DB，个个都要full-stack。DynamoDB就属于Developer完全可以搞定的，
不需要DBA或者SRE帮忙。 一般业务不需要多表join的，DynamoDB设计好了都没问题，还是有些dev说，我们的业务需要ACID，今天就来打脸了，Transaction is not a deal breaker for Dynamodb anymore。

DynamoDB Transaction保证ACID。ACID意思是atomicity, consistency, isolation, and durability。这里主要谈的是atomicity，同时操作
多个表或者行，结果要是all or nothing，全部操作成功，或者什么都不做。

### 两个Transactional API
* TransactWriteItems
    * 一个transaction最多操作25个不用的items，每个item大小不超过400 KB，整个transaction大小不超过4 MB。
    * 同一个transaction只能操作同一个item一次。
    * 消耗双倍WCUs。
    * 可以用client token来保持幂等(10 mins)。

   
* TransactReadItems
    * 一个transaction最多操作25个不用的GET，整个transaction大小不超过4 MB。
    * 返回的是同步读的结果。
    * 消耗双倍RCUs。
    * 读写都不用锁，有冲突就会取消transaction，需要自己处理retry。注意，即使但transaction取消了，还是消耗同样的capacity units。
   
* 其他：
    * transaction读写都不用锁，有冲突就会取消transaction，需要自己处理retry。注意，即使但transaction取消了，还是消耗同样的capacity units。
    * transaction必须在同一个AWS region
    * transaction必须在同一个AWS 账户
    * transaction必须在DynamoDB里

### 例子

源码： https://github.com/shubozhang/micronaut-with-java/blob/main/mn-dynamodb/src/test/java/com/sz/transaction/TransactionTest.java

##### 建立三个table
1.Table Account: unique accountId
```java
@NoArgsConstructor
@AllArgsConstructor
@DynamoDBTable(tableName="Account")
@ToString
@Data
public class Account {
    private String accountId;
    @DynamoDBHashKey(attributeName = "AccountId")
    public String getAccountId() { return accountId; }
    public void setAccountId(String accountId) { this.accountId = accountId; }
}
```
2. Table Room：unique RoomId，属性available只能是YES或者NO
```java
@NoArgsConstructor
@AllArgsConstructor
@DynamoDBTable(tableName="Room")
@ToString
@Data
public class Room {
    private String roomId;
    private String available;

    public Room(String roomId) { this.roomId = roomId; }
    @DynamoDBHashKey(attributeName = "RoomId")
    public String getRoomId() { return roomId; }
    public void setRoomId(String roomId) { this.roomId = roomId; }
    @DynamoDBAttribute(attributeName = "Available")
    public String getAvailable() { return available; }
    public void setAvailable(String available) { this.available = available; }
}
```   
3. Table Order：unique orderId，accountId必须是存在的，roomId也必须是存在的，并且对应的room available必须是之前YES， 之后NO
```java
@NoArgsConstructor
@AllArgsConstructor
@DynamoDBTable(tableName="Order")
@ToString
@Data
public class Order {
    private String orderId;
    private String accountId;
    private String roomId;

    public Order(String orderId) { this.orderId = orderId; }

    @DynamoDBHashKey(attributeName = "OrderId")
    public String getOrderId() { return orderId; }
    public void setOrderId(String orderId) { this.orderId = orderId; }

    @DynamoDBAttribute(attributeName = "AccountId")
    public String getAccountId() { return accountId; }
    public void setAccountId(String accountId) { this.accountId = accountId;}

    @DynamoDBAttribute(attributeName = "RoomId")
    public String getRoomId() { return roomId; }
    public void setRoomId(String roomId) { this.roomId = roomId; }
}
```

##### 写Transaction
1. Create an order
```
    private String createOrder(Order order) {
        // 1。 先检查accountId
        DynamoDBTransactionWriteExpression checkAccount = new DynamoDBTransactionWriteExpression()
                .withConditionExpression("attribute_exists(AccountId)");

        // 2。检查Room是不是available，操作之前的status必须是YES。
        Map<String, AttributeValue> roomUpdateValues = new HashMap<>();
        roomUpdateValues.put(":pre_status", new AttributeValue("YES"));
        DynamoDBTransactionWriteExpression checkRoom = new DynamoDBTransactionWriteExpression()
                .withExpressionAttributeValues(roomUpdateValues)
                .withConditionExpression("Available = :pre_status");
        
        // 3。检查没有重复的order
        DynamoDBTransactionWriteExpression checkOrder = new DynamoDBTransactionWriteExpression()
                .withConditionExpression("attribute_not_exists(OrderId)");

        TransactionWriteRequest transactionWriteRequest = new TransactionWriteRequest();
        transactionWriteRequest.addConditionCheck(new Account(order.getAccountId()), checkAccount);
        // 更新房间状态，预定后变为NO
        transactionWriteRequest.addUpdate(new Room(order.getRoomId(), "NO"), checkRoom);
        transactionWriteRequest.addPut(order, checkOrder);

        // 4。 写Transaction
        return executeTransactionWrite(transactionWriteRequest);
    }
```
2. Execute Transaction Write
```
    private static String executeTransactionWrite(TransactionWriteRequest transactionWriteRequest) {
        try {
            mapper.transactionWrite(transactionWriteRequest);
        } catch (DynamoDBMappingException ddbme) {
            return ("Client side error in Mapper, fix before retrying. Error: " + ddbme.getMessage());
        } catch (ResourceNotFoundException rnfe) {
            return ("One of the tables was not found, verify table exists before retrying. Error: " + rnfe.getMessage());
        } catch (InternalServerErrorException ise) {
            return ("Internal Server Error, generally safe to retry with back-off. Error: " + ise.getMessage());
        } catch (TransactionCanceledException tce) {
            // 重点测试这个exception，它返回的信息会准确告诉我们哪一步有错误
            return ("Transaction Canceled, implies a client issue, fix before retrying. Error: " + tce.getMessage());
        } catch (Exception ex) {
            return ("An exception occurred, investigate and configure retry strategy. Error: " + ex.getMessage());
        }
        return "SUCCESS";
    }
```

##### 读Transaction
1. Read an order：读出order，和相关的ROOM，ACCOUNT信息
```
    private List<Object> readOrder(Order order) {
        TransactionLoadRequest transactionLoadRequest = new TransactionLoadRequest();
        transactionLoadRequest.addLoad(order);
        transactionLoadRequest.addLoad(new Room(order.getRoomId()));
        transactionLoadRequest.addLoad(new Account(order.getAccountId()));

        return executeTransactionLoad(transactionLoadRequest);
    }
```
2. Execute Transaction Load
```
    private static List<Object> executeTransactionLoad(TransactionLoadRequest transactionLoadRequest) {
        List<Object> loadedObjects = new ArrayList<Object>();
        try {
            loadedObjects = mapper.transactionLoad(transactionLoadRequest);
        } catch (DynamoDBMappingException ddbme) {
            System.err.println("Client side error in Mapper, fix before retrying. Error: " + ddbme.getMessage());
        } catch (ResourceNotFoundException rnfe) {
            System.err.println("One of the tables was not found, verify table exists before retrying. Error: " + rnfe.getMessage());
        } catch (InternalServerErrorException ise) {
            System.err.println("Internal Server Error, generally safe to retry with back-off. Error: " + ise.getMessage());
        } catch (TransactionCanceledException tce) {
            System.err.println("Transaction Canceled, implies a client issue, fix before retrying. Error: " + tce.getMessage());
        } catch (Exception ex) {
            System.err.println("An exception occurred, investigate and configure retry strategy. Error: " + ex.getMessage());
        }
        return loadedObjects;
    }
```

##### 准备测试数据
```
    // 插入5个account，A1。。。A5
    private static void insertAccounts(Integer count) {
        AccountDao accountDao = new AccountDao();
        for (int i = 0; i < count; i++) {
            accountDao.saveItem(new Account("A"+i));
        }
    }
    // 插入5个room，R1。。。R5，初始状态是YES
    private static void insertRooms(Integer count) {
        RoomDao roomDao = new RoomDao();
        for (int i = 0; i < count; i++) {
            roomDao.saveItem(new Room("R"+i, "YES"));
        }
    }
```

##### 测试一：完成order
```
@Test
public void testCreateOrder() {
    // 新建order
    Order order = new Order();
    order.setOrderId("1");
    order.setAccountId("A1");
    order.setRoomId("R1");

    // 创建order
    createOrder(order);

    // 读order
    List<Object> objects = readOrder(order);
    Order resOrder = (Order) objects.get(0);
    Room resRoom = (Room) objects.get(1);
    Account resAccount = (Account) objects.get(2);

    // 检查结果
    assertThat(resOrder).isEqualTo(order);
    assertThat(resAccount).isEqualTo(new Account(order.getAccountId()));
    assertTrue(resRoom.getRoomId().equals(order.getRoomId())
            && resRoom.getAvailable().equals("NO"));

    // clean up
    deleteOrder(order);
    }
```

##### 测试二：无效account
```
    @Test
    public void testInvalidAccountException() {
        Order order = new Order();
        order.setOrderId("2");
        order.setAccountId("A");
        order.setRoomId("R3");

        String res = createOrder(order);
        assertTrue(res.contains("Transaction Canceled, implies a client issue, fix before retrying. Error: "));
        // 第一个conditionCheck就是accountId
        assertTrue(res.contains("[ConditionalCheckFailed, None, None]"));
    }
```


##### 测试三：无效room
```
    @Test
    public void testInvalidRoomException() {
        Order order = new Order();
        order.setOrderId("2");
        order.setAccountId("A2");
        order.setRoomId("R");

        String res = createOrder(order);
        assertTrue(res.contains("Transaction Canceled, implies a client issue, fix before retrying. Error: "));
        // 第二个conditionCheck是Room
        assertTrue(res.contains("[None, ConditionalCheckFailed, None]"));
    }
```


##### 测试四：重复room
```
    @Test
    public void testDupRoomException() {
        Order order = new Order();
        order.setOrderId("2");
        order.setAccountId("A2");
        order.setRoomId("R2");

        String res = createOrder(order);
        assertTrue(res.equals("SUCCESS"));

        Order order2 = new Order();
        order2.setOrderId("3");
        order2.setAccountId("A3");
        order2.setRoomId("R2");

        String res2 = createOrder(order2);
        assertTrue(res2.contains("Transaction Canceled, implies a client issue, fix before retrying. Error: "));
        // 还是room conditionCheck 失败
        assertTrue(res2.contains("[None, ConditionalCheckFailed, None]"));

        // clean up
        deleteOrder(order);
    }
```


##### 测试五：重复order
```
    @Test
    public void testDupOrderException() {
        Order order = new Order();
        order.setOrderId("2");
        order.setAccountId("A2");
        order.setRoomId("R2");

        String res = createOrder(order);
        assertTrue(res.equals("SUCCESS"));

        Order order2 = new Order();
        order2.setOrderId("2");
        order2.setAccountId("A3");
        order2.setRoomId("R3");

        String res2 = createOrder(order2);
        assertTrue(res2.contains("Transaction Canceled, implies a client issue, fix before retrying. Error: "));
        // order conditionCheck 失败
        assertTrue(res2.contains("[None, None, ConditionalCheckFailed]"));

        // clean up
        deleteOrder(order);
    }
```

##### 测试六：全无效
```
    @Test
    public void testAllInvalidException() {
        Order order = new Order();
        order.setOrderId("2");
        order.setAccountId("A2");
        order.setRoomId("R2");

        String res = createOrder(order);
        assertTrue(res.equals("SUCCESS"));

        Order order2 = new Order();
        order2.setOrderId("2");
        order2.setAccountId("NONE");
        order2.setRoomId("NONE");

        String res2 = createOrder(order2);
        assertTrue(res2.contains("Transaction Canceled, implies a client issue, fix before retrying. Error: "));
        // 所有conditionCheck都失败
        assertTrue(res2.contains("[ConditionalCheckFailed, ConditionalCheckFailed, ConditionalCheckFailed]"));

        // clean up
        deleteOrder(order);
    }
```

### 更多例子
* https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBMapper.Transactions.html


