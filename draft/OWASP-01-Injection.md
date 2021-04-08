# OWASP Top 10- 2017
 
OWASP stands for the open web application security project.

## OWASP Top-10 Secure Programming Modules
* **Injection**
* Broken Authentication
* Sensitive Data Exposure
* XML External Entities (XXE)
* Broken Access Control
* Security Misconfiguration
* Cross-Site Scripting (XSS)
* Insecure Deserialization
* Using Components with Known Vulnerabilities
* Insufficient Logging and Monitoring


### Injection
Injection flaws, such as SQL, NoSQL, OS, and LDAP injection, occur when untrusted data is sent
to an interpreter as part of a command or query. The attacker's hostile data can trick the interpreter
into executing unintended commands or accessing data without proper authorization.

##### What should you know about injection 

consider anyone who can send untrusted data to the system, 
including external users internal users and administrators.  

Injection flaws occur when an attacker sent hospital data to interpretive.

Injection flaws are very prevalent, particularly in legacy code injection vulnerabilities are all.

SQL, LDAP, XPath, os commands, XML parsers,  smtp headers, expression languages and orm queries.

Injection flaws are easy to discover when examining code scanners and buzzers can help attackers 
find injection laws.

Injection can result in data loss direction or disclosure to unauthorized parties loss of 
accountability or denial of access 
 
Injection can sometimes lead to complete post take over the business impact depends on the needs 
of the application and data.

##### Is the Application Vulnerable
 An application is vulnerable to injection attack when user-supplied data is not validated 
 filtered or sanitized by the application.

 Dynamic queries or non parameterized calls without context aware escaping are used directly 
 in the interpreter.

 Hostile data issues with an object relational mapping orm search parameters to extract 
 additional sensitive records. 
 
Hostile data is directly used or concatenated it such that the SQL or command contains 
both structure and hospital data in dynamic queries commands, or stored procedures.

 Some of the more common injections are SQL, NoSQL,  os command, object relational mapping
 orm , LDAP and expression language El or object graph navigation library OGNL injection.

 The concept is identical among all interpreters.


 source code review is the best method of detecting if applications are vulnerable to injections 
 closely followed by thorough automated testing about parameters headers, URL, cookies,  Jason, soap, xml data inputs.


 organizations can include static source (SAST) and dynamic application test (DAST) tools into the ci 
 CD pipeline to identify newly introduced rejection was prior to production deployment.

##### Example Attack Scenarios

scenario one 

The application uses untrusted data in the construction of the following vulnerable  SQL call:

```
String query = "SELECT * FROM accounts WHERE custID='"+ request.getParameter("id") + "'";
```


scenario two 
Similarly, an application's blind trust in frameworks may result in queries that are still vulnerable, 
for example, hibernate query language well.

```
Query HQLQuery = session.createQuery("FROM accounts WHERE custID='"+request.getParameter("id")+"'");
```


In both cases, the attacker modifies the 'id' parameter value in her browser to send: `' or '1'='1`

```
example: http://example.com/app/accountView?id=' or '1'='1
```

This changes the meaning of both queries to return all the records from the accounts table 
 more dangerous attacks could modify data or even invoke stored procedures.

##### How Do I Prevent Inject

Injection requires keeping untrusted data, separate from commands and queries 

* The preferred option is to use the safe API, which avoid use of the interpreter entirely 
* provides parameterized interface.


 Be careful with APIs such as stored procedures that parameterized can still 
 introduce injection under the hood.


If parameterized API is not available, you should carefully escape special characters using the 
specific escape syntax for that interpreter.

OWASPs ESAPI provides many of these escape routes.


Positive input validation is also recommended but it's not a complete Defense as many 
applications require special characters in their input.


 You can also use LIMIT and other SQL controls within queries to prevent mass disclosure of records 
 in case of sql injection.
