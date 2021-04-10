---
title: "OWASP 十大网站安全风险 （一）： 注入攻击"
date: 2021-04-07
hero: /images/posts/pg.svg
menu:
    sidebar:
        name: owasp-1-sql-injection
        identifier: owasp-1
        parent: tools
        weight: 10
---

OWASP是 open web application security project 的缩写。这个系列主要介绍这十个最多被攻击的安全漏洞。

## OWASP 十大信息安全主题
> 1. **注入攻击 （Injection）**
> 2. 无效身份认证（Broken Authentication）
> 3. 敏感信息泄漏（Sensitive Data Exposure）
> 4. XML外部处理器漏洞（XML External Entities (XXE)）
> 5. 无效存取控管（Broken Access Control）
> 6. 错误设置安全系统（Security Misconfiguration）
> 7. 跨站攻击（Cross-Site Scripting (XSS)）
> 8. 不安全的反序列化漏洞（Insecure Deserialization）
> 9. 使用已有漏洞元件（Using Components with Known Vulnerabilities）
> 10. 日志和监控不足风险（Insufficient Logging and Monitoring）


## 注入攻击
注入攻击是指在SQL, NoSQL, 系统命令, 和 LDAP等注入时候, 非安全的数据作为命令或者语句传入到解释器中，这些攻击者的
敌对数据可以骗过解释器，在没有授权的情况下执行一些危险命令或者取得数据

### 了解注入攻击
* 首先要考虑到，外部用户， 内部用户， 管理员等都可能给系统发送非安全信息。当攻击者给解释器发送敌对数据的时候，注入
攻击就发生了。

* 注入漏洞是非常普遍的，尤其是老旧系统。 最可能出现的地方包括SQL, LDAP, XPath, OS 命令, XML parsers, 
smtp headers, Expression languages， ORM语句， OGNL（object graph navigation library）等等.

* 通过检查代码是很容见发现注入漏洞的。攻击者经常用扫描软件或者模糊测试软件去找注入漏洞。

* 注入漏洞能导致很多严重后果，像数据丢失，数据损坏，数据泄漏，数据授权被破坏，有时候可能导致整个系统完全被攻击之掌控

### 什么样的应用程序容易被攻击
* 程序对用户提交的数据不验证，不过滤，不清理。
* 动态SQL语句，或者非参数注入语句没有对特殊符号处理就直接传入解释器。
* 非验证数据直接用在ORM搜索参数里面
* 非验证数据直接用在SQL， 系统命令，动态SQL， 或者存储过程里

预防注入攻击的最好方法是代码审查，然后是对下面等数据输入进行严格测试：
* Parameters
* Headers
* URL
* Cookies
* JSON
* SOAP
* XML

现在公司基本会在CI/CD pipeline加入一些扫描，测试软件来发现这些漏洞。


### 注入漏洞攻击实例
例子一：
程序使用非验证数据去创建SQL语句
```
String query = "SELECT * FROM accounts WHERE custID='"+ request.getParameter("id") + "'";
```


例子二：
程序使用非验证数据去创建HQL语句
```
Query HQLQuery = session.createQuery("FROM accounts WHERE custID='"+request.getParameter("id")+"'");
```

在上面两个例子中，攻击者可以在浏览器篡改`id`参数， 发送`' or '1'='1`

```
Example: http://example.com/app/accountView?id=' or '1'='1
```
这样上面两个query会返回accounts表里所有数据。所以可以看出，注入攻击是非常严重，完全可以取得， 破坏数据。


### 预防注入漏洞
* 要把非验证数据和queries， 系统命令之类的非开
* 使用安全API, 完全避免使用解释器
* 使用参数化接口
* 如果没有参数化API的话，我们必须在解释器里很小心去处理特殊符号。
* 可以积极的对输入数据进行验证，但很多系统要求特殊符号作为输入，这样就很难完全检查到注入漏洞了。
* 使用关键字LIMIT或者其他SQL query控制，避免返回大量数据。
