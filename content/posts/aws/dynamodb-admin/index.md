---
title: "介绍一个本地DynamoDB图形界面应用: dynamodb-admin"
date: 2021-03-16
menu:
    sidebar:
        name: dynamodb-admin
        identifier: dynamodb-admin
        parent: aws
        weight: 10
hero: hero.svg
---

DynamoDB一直被人诟病没有好的query tool，查询数据很麻烦，到现在也没有。在就是本地开发不是很友好，AWS提供了Dynamodb Local，但是aws cli query数据
还是很麻烦。今天推荐一个给DynamoDB Local做的Graphical User Interface (GUI)---dynamodb-admin，可以像在AWS console一样，让本地数据可视化和操作化。

### 快速设置

1. 通过npm安装dynamodb-admin

    ```node
    npm install -g dynamodb-admin
    ```

2. 运行docker： local dynamodb

    `docker-compose.yml` file
    ```dockerfile
    version: '3.8'
    services:
      dynamodb-local:
        command: "-jar DynamoDBLocal.jar -sharedDb -optimizeDbBeforeStartup -dbPath ./data"
        image: "amazon/dynamodb-local:latest"
        container_name: dynamodb-local
        ports:
          - "8000:8000"
        volumes:
          - "./docker/dynamodb:/home/dynamodblocal/data"
        working_dir: /home/dynamodblocal
    ```
    
    Run `docker-compose.yml` file
    ```
    docker-compose up -d
    ```

3. 启动dynamodb-admin
    * MacOS：
        ```
        dynamodb-admin
      
        // 或者定义region和credentials
        AWS_REGION=eu-west-1 AWS_ACCESS_KEY_ID=local AWS_SECRET_ACCESS_KEY=local dynamodb-admin
        ``` 
    * Windows: 
        ```
        export DYNAMO_ENDPOINT=http://localhost:8000
        dynamodb-admin
        ```

4. 完成
   * DynamoDB 运行在 `http://localhost:8000`
   * 图形界面运行在 `http://localhost:8001`


### UI测试
我通过aws cli command新建了两个table， 来看看UI。

1. 首页比较简洁，显示两个表。

    ![Alt text](/images/posts/aws/dynamodb-admin/tables.png)

2. 选择Music table，显示items。可以GET，SCAN，QUERY数据。

    ![Alt text](/images/posts/aws/dynamodb-admin/items.png)
   
3. 从UI QUERY GSI 

    ![Alt text](/images/posts/aws/dynamodb-admin/search.png)

4. Meta 可以显示schema和表的一些统计
   
    ![Alt text](/images/posts/aws/dynamodb-admin/schema.png)
    
5. 可以通过UI建表   
   
    ![Alt text](/images/posts/aws/dynamodb-admin/create-table.png)

支持本地CRUD测试应该足够用了。

### CRUD测试
建立一个简单的Micronaut webapp来测试CRUD DynamoDB table。

源码：https://github.com/shubozhang/micronaut-with-java/blob/main/mn-dynamodb/

1. 通过AWS CLI创建一个Music Table，Table包括一个本地索引和全局索引 

    ```bash
    aws dynamodb create-table --cli-input-json file://music_table.json --endpoint-url http://localhost:8000
    ```
   
    `music_table.json`:
    ```json
    {
      "AttributeDefinitions": [
        { "AttributeName": "Artist", "AttributeType": "S"},
        { "AttributeName": "SongTitle", "AttributeType": "S"},
        { "AttributeName": "AlbumTitle", "AttributeType": "S"},
        { "AttributeName": "Upc", "AttributeType": "S"}
      ],
      "TableName": "Music",
      "KeySchema": [
        {
          "KeyType": "HASH",
          "AttributeName": "Artist"
        },
        {
          "KeyType": "RANGE",
          "AttributeName": "SongTitle"
        }
      ],
      "LocalSecondaryIndexes": [
        {
          "IndexName": "lsi_album",
          "KeySchema": [
            { "AttributeName": "Artist", "KeyType": "HASH"},
            { "AttributeName": "AlbumTitle", "KeyType": "RANGE"}
          ],
          "Projection": {
            "ProjectionType": "ALL"
          }
        }
      ],
      "ProvisionedThroughput": {
        "ReadCapacityUnits": 3,
        "WriteCapacityUnits": 3
      },
      "GlobalSecondaryIndexes": [
        {
          "IndexName": "gsi_upc",
          "KeySchema": [
            { "AttributeName": "Upc", "KeyType": "HASH"}
          ],
          "Projection": {
            "ProjectionType": "ALL"
          },
          "ProvisionedThroughput": {
            "ReadCapacityUnits": 3,
            "WriteCapacityUnits": 3
          }
        }
      ]
    }
    ```


2. Create a model for Music Table

    ```java
    @DynamoDBTable(tableName="Music")
    @ToString
    public class Music {
    private String artist;
    private String songTitle;
    private String albumTitle;
    private Integer year;
    private String upc;
    
        @DynamoDBHashKey(attributeName="Artist")
        public String getArtist() { return artist;}
        public void setArtist(String artist) {this.artist = artist;}
    
        @DynamoDBRangeKey(attributeName="SongTitle")
        public String getSongTitle() { return songTitle;}
        public void setSongTitle(String songTitle) {this.songTitle = songTitle;}
    
        @DynamoDBIndexRangeKey(attributeName = "AlbumTitle", localSecondaryIndexName = "lsi_album")
        public String getAlbumTitle() { return albumTitle;}
        public void setAlbumTitle(String albumTitle) {this.albumTitle = albumTitle;}
    
        @DynamoDBAttribute(attributeName = "Year")
        public Integer getYear() { return year; }
        public void setYear(Integer year) { this.year = year; }
    
        @DynamoDBIndexHashKey(attributeName = "Upc", globalSecondaryIndexName= "gsi_upc")
        public String getUpc() { return upc; }
        public void setUpc(String upc) { this.upc = upc; }
    }
    ```

    * `PartitionKey`是由`HashKey Artist`和`RangeKey SongTitle`组成。
    * 建立`LSI IndexRangeKey AlbumTitle`。它将和`HashKey Artist`一起用。
    * 建立`GSI IndexHashKey Upc`。它可以单独引用。
    
3. 建立一个DBMapper class。设置client， mapper，和index

    ```java
    public class DBMapper {
        private static final String TABLE_NAME = "Music";
        private static final String LSI_ALBUM_INDEX_NAME =  "lsi_album";
        private static final String GSI_UPC_INDEX_NAME = "gsi_upc";
        private static AmazonDynamoDB client;
        private static DynamoDBMapper mapper;
        private static Index lsi_album;
        private static Index gsi_upc;
    
        static {
            AWSCredentialsProvider creds = new AWSCredentialsProvider() {
                @Override
                public AWSCredentials getCredentials() { return new BasicAWSCredentials("local", "local"); }
                @Override
                public void refresh() { }
            };
            client = AmazonDynamoDBClientBuilder.standard()
                    .withCredentials(creds)
                    .withEndpointConfiguration(new AwsClientBuilder.EndpointConfiguration("http://localhost:8000", "us-east-1"))
                    .build();
            mapper = new DynamoDBMapper(client);
            DynamoDB dynamoDb = new DynamoDB(client);
            Table table = dynamoDb.getTable(TABLE_NAME);
            lsi_album = table.getIndex(LSI_ALBUM_INDEX_NAME);
            gsi_upc = table.getIndex(GSI_UPC_INDEX_NAME);
    
        }
        public static DynamoDBMapper getMapper(){ return mapper; }
        public static Index getLSIAlbumTitle() { return lsi_album; }
        public static Index getGSIUpc() { return gsi_upc; }
    }
    ```

4. Create a MusicDao for CRUD
   
   ```java
   @Singleton
   public class MusicDao {
       private final DynamoDBMapper mapper = DBMapper.getMapper();
       private final Index lsiAlbum = DBMapper.getLSIAlbumTitle();
       private final Index gsiAlbum = DBMapper.getGSIUpc();
   
       public Music getItem(Music music) { return mapper.load(music); }
       public void saveItem(Music music) { mapper.save(music); }
       public void deleteItem(Music music) { mapper.delete(music); }
   
       public List<Music> queryItemByLSI(Music music) throws JsonProcessingException {
           List<Music> musicList = new ArrayList<>();
           QuerySpec spec = new QuerySpec()
                   .withKeyConditionExpression("Artist = :v_artist and AlbumTitle = :v_title")
                   .withValueMap(new ValueMap()
                           .withString(":v_artist", music.getArtist())
                           .withString(":v_title", music.getAlbumTitle()));
   
           ItemCollection<QueryOutcome> items = lsiAlbum.query(spec);
           Iterator<Item> itemsIter = items.iterator();
           while (itemsIter.hasNext()) {
               Item item = itemsIter.next();
               ObjectMapper mapper = new ObjectMapper();
               mapper.setPropertyNamingStrategy(PropertyNamingStrategy.UPPER_CAMEL_CASE);
               musicList.add(mapper.readValue(item.toJSON(), Music.class));
           }
           return musicList;
       }
   
       public List<Music> queryItemByGSI(Music music) throws JsonProcessingException {
           List<Music> musicList = new ArrayList<>();
           QuerySpec spec = new QuerySpec()
                   .withKeyConditionExpression("Upc = :v_upc")
                   .withValueMap(new ValueMap().withString(":v_upc", music.getUpc()));
           ItemCollection<QueryOutcome> items = gsiAlbum.query(spec);
           Iterator<Item> itemsIter = items.iterator();
           while (itemsIter.hasNext()) {
               Item item = itemsIter.next();
               ObjectMapper mapper = new ObjectMapper();
               mapper.setPropertyNamingStrategy(PropertyNamingStrategy.UPPER_CAMEL_CASE);
               musicList.add(mapper.readValue(item.toJSON(), Music.class));
           }
           return musicList;
       }
   }
   ```


5. Controller for CRUD 

   ```java
   
   @Controller("/music")
   public class MusicController {
       private static final Logger log = LoggerFactory.getLogger(MusicController.class);
       @Inject
       MusicDao musicDao;
       @Inject
       ObjectMapper mapper;
       
       @Get("{?music*}") // this is used to mapping request parameters
       @Produces(MediaType.APPLICATION_JSON)
       public HttpResponse getEvent(Music music) {
           Music item = musicDao.getItem(music);
           if (item == null) {
               final ItemError notFound = ItemError.builder()
                       .status(HttpStatus.NOT_FOUND.getCode())
                       .error(HttpStatus.NOT_FOUND.name())
                       .message("item not found")
                       .build();
               return HttpResponse.notFound(notFound);
           }
           return HttpResponse.ok(item);
       }
   
       @Get("/query/lsi{?music*}")
       @Produces(MediaType.APPLICATION_JSON)
       public HttpResponse getEventByLSI(Music music) throws JsonProcessingException {
           List<Music> items = musicDao.queryItemByLSI(music);
           if (items.isEmpty()) {
               final ItemError notFound = ItemError.builder()
                       .status(HttpStatus.NOT_FOUND.getCode())
                       .error(HttpStatus.NOT_FOUND.name())
                       .message("item not found")
                       .build();
               return HttpResponse.notFound(notFound);
           }
           return HttpResponse.ok(items);
       }
   
       @Get("/query/gsi{?music*}")
       @Produces(MediaType.APPLICATION_JSON)
       public HttpResponse getEventByGSI(Music music) throws JsonProcessingException {
           List<Music> items = musicDao.queryItemByGSI(music);
           if (items.isEmpty()) {
               final ItemError notFound = ItemError.builder()
                       .status(HttpStatus.NOT_FOUND.getCode())
                       .error(HttpStatus.NOT_FOUND.name())
                       .message("item not found")
                       .build();
               return HttpResponse.notFound(notFound);
           }
           return HttpResponse.ok(items);
       }
   
       @Post
       @Produces(MediaType.APPLICATION_JSON)
       public HttpResponse saveEvent(@Body String body) throws JsonProcessingException {
           musicDao.saveItem(mapper.readValue(body, Music.class));
           return HttpResponse.ok();
       }
   
       @Delete
       @Produces(MediaType.APPLICATION_JSON)
       public HttpResponse deleteEvent(@Body String body) throws JsonProcessingException {
           musicDao.deleteItem(mapper.readValue(body, Music.class));
           return HttpResponse.ok();
       }
   }
   ```

6. 终于到了测试了
  
   建立几个helper method：

   ```java
       private void saveItem(final Music music) throws JsonProcessingException {
           final String body = mapper.writeValueAsString(music);
           client.toBlocking().exchange(HttpRequest.POST("/", body));
       }
       // getItem要用partitionKey
       private Music getItem(final Music music) {
           final String uri = new UriTemplate("/{?artist,songTitle}").expand(music);
           return client.toBlocking().retrieve(HttpRequest.GET(uri), Music.class);
       }
       // LSI和GSI需要用scan或者query
       private String queryItemsByLSI(Music music) {
           final String uri = new UriTemplate("/query/lsi{?artist,albumTitle}").expand(music);
           return client.toBlocking().retrieve(HttpRequest.GET(uri));
       }
       private String queryItemsByGSI(Music music) {
           final String uri = new UriTemplate("/query/gsi{?upc}").expand(music);
           return client.toBlocking().retrieve(HttpRequest.GET(uri));
       }
       private void deleteItem(final Music music) throws JsonProcessingException {
           final String delBody = mapper.writeValueAsString(music);
           client.toBlocking().exchange(HttpRequest.DELETE("/",delBody));
       }
   ```
   测试：
   ```java
    @Test
    void testGetEventByPartitionKey() throws JsonProcessingException {
        Music music = new Music("Beyond", "Love", "Love", 1993, "09000");
        saveItem(music);
        // HashKey "Beyond", RangeKey "Love"
        Music requestMusic = new Music("Beyond", "Love", null, null, null);
        Music result = getItem(requestMusic);

        assertTrue(result.getArtist().equals("Beyond"));
        assertTrue(result.getAlbumTitle().equals("Love"));
        assertTrue(result.getYear() == 1993);
    }

    @Test
    void testGetEventByLSI() throws JsonProcessingException {
        Music music = new Music("Maroon 5", "One More Night", "Overexposed", 2012, "20120123");
        saveItem(music);
        Music music1 = new Music("Maroon 5", "Payphone", "Overexposed", 2012, "20120124");
        saveItem(music1);
        // HashKey: "Maroon 5", LSI RangeKey: "Overexposed"
        Music requestMusic = new Music("Maroon 5", null, "Overexposed", null, null);
        String musics = queryItemsByLSI(requestMusic);
        List<Music> musicList;

        musicList = mapper.readValue(musics, new TypeReference<>() {});
        Collections.sort(musicList, Comparator.comparing(Music::getUpc));
        assertTrue(musicList.size() == 2);
        assertTrue(musicList.get(0).getUpc().equals("20120123"));
        assertTrue(musicList.get(1).getUpc().equals("20120124"));
    }

    @Test
    void testGetEventByGSI() throws JsonProcessingException {
        Music music = new Music("MJ", "Dangerous", "Dangerous", 1991, "MJ-19910123");
        saveItem(music);
        // GSI HashKey: "MJ-19910123"
        Music requestMusic = new Music(null, null, null, null, "MJ-19910123");
        String musics = queryItemsByGSI(requestMusic);
        List<Music> musicList;

        musicList = mapper.readValue(musics, new TypeReference<>() {});
        assertTrue(musicList.size() == 1);
        assertTrue(musicList.get(0).getUpc().equals("MJ-19910123"));
        assertTrue(musicList.get(0).getArtist().equals("MJ"));
        assertTrue(musicList.get(0).getSongTitle().equals("Dangerous"));
        assertTrue(musicList.get(0).getAlbumTitle().equals("Dangerous"));
        assertTrue(musicList.get(0).getYear().equals(1991));
    }
    
    @Test
    void testDeleteEvent() throws JsonProcessingException {
        Music music = new Music("Robin", "Angel", "Angel", 2000, "08000");
        saveItem(music);
        Music delRequest = new Music("Robin", "Angel", null, null, null);
        deleteItem(delRequest);

        Music request = new Music("Robin", "Angel", null, null, null);
        try {
            getItem(request);
        } catch (HttpClientResponseException e) {
            assertEquals(HttpStatus.NOT_FOUND, e.getResponse().getStatus());
            Optional<ItemError> jsonError = e.getResponse().getBody(ItemError.class);
            assertTrue(jsonError.isPresent());
            log.debug("Custom Error: {}", jsonError.get());
            assertEquals(404, jsonError.get().getStatus());
            assertEquals("NOT_FOUND", jsonError.get().getError());
            assertEquals("item not found", jsonError.get().getMessage());
        }

    }
   ```


### 参考
* https://github.com/aaronshaf/dynamodb-admin
* https://github.com/instructure/dynamo-local-admin-docker
