---
title: "OWASP 十大网站安全风险 （四）：XML外部处理器漏洞"
date: 2021-04-30
hero: owasp-top-ten.jpg
menu:
    sidebar:
        name: owasp-4-XML-External-Entities (XXE)
        identifier: owasp-4
        parent: tools
        weight: 10
---

## OWASP 十大信息安全主题
> 1. 注入攻击 （Injection）
> 2. 无效身份认证（Broken Authentication
> 3. 敏感信息泄漏（Sensitive Data Exposure）
> 4. **XML外部处理器漏洞（XML External Entities (XXE)）**
> 5. 无效存取控管（Broken Access Control）
> 6. 错误设置安全系统（Security Misconfiguration）
> 7. 跨站攻击（Cross-Site Scripting (XSS)）
> 8. 不安全的反序列化漏洞（Insecure Deserialization）
> 9. 使用已有漏洞元件（Using Components with Known Vulnerabilities）
> 10. 日志和监控不足风险（Insufficient Logging and Monitoring）



## XML外部处理器漏洞
Many older or poorly configured XML processors evaluate external entity reference within XML documents.
External entities can be used to disclose internal files using the file URI handler, internal file shares
, internal port scanning, remote code execution, and denial of service attacks.



### 了解XML外部处理器漏洞

attackers can exploit vulnerable xml processors if they can upload xml or include hostile content in an xml document 
exploiting vulnerable code dependencies or integrations. 

By default many older xml processors a while specifications of an external entity, URI, that is the reference 

and evaluated during xml processing.


SAST tools can discover this issue by inspecting dependencies and configuration.

DAST tools require additional manual steps to detect and exploit this issue. Manual testers need to be trained in how 
to test for XXE, as it is not commonly tested as of 2017.


These flaws, can be used to extract data ,executed remote request from the server , scan internal systems and or perform 
a denial of service attack, as well as executed other attacks 

The business impact depends on the protections of all affected applications and data.


### Is your application Vulnerable
Applications, and in particular xml based web services for downstream integrations might be vulnerable to attack if the 
application, except xml directly for xml uploads, especially from untrusted sources for inserts untrusted data into xml 
documents which is then parsed by an xml processor.


Any of the xml processors in the application or soap based web services has DTD or document type definitions enabled.


As the exact mechanism for disabling DTD processing varies by processor, it is good practice to consult a reference, 
such as the OWASP cheat sheet 'XXE prevention'.


If your application uses SAML for identity processing within federated security or single sign on sso purposes.
 SAML uses xml for identity assertions, and may be vulnerable.


If the application uses soap to version 1.2, it is likely susceptible to XXE attacks if xml entities so being passed 
through soap framework.


Being vulnerable to XXE attacks likely means that the application is vulnerable to denial of service attacks, 
including the billion laughs attack.

### Examples

Numerous public execs the issues have been discovered instantly attacking and then devices and XXE occurs in a lot of 
unexpected places, including deeply nested dependencies, the easiest way is to upload malicious xml file accepted.


##### scenario one the attacker attempts to extract data from the server 

##### scenario to an attack improves the servers private 
network by changing the below entity on 

##### scenario three an attacker attempts a denial of service attack by including a potentially endless file.



### How to prevent this
Developer training is essential to identify and mitigate XXE.


Besides that prevented XXE requires that you whenever possible use less complex data formats, such as Jason and avoid 
serialization of sensitive data.


Patch or upgrade all xml processors and libraries in use by the application or on the underlying operating system.

Use dependency checkers.

Update soap to SOAP 1.2 or higher.


Disable xml external entity DTD processing in all xml horses in the application , as per the OWASP cheat sheet 'XXE prevention'.

Implement positive server-side input validation, filtering, or sanitization to prevent hostile data within xml documents, headers,  or nodes.


Verify that xml or XSL file upload functionality validates incoming xml using XSD validation or similar.


SAST tools can help detect XXE in source code, although manual code review is the best alternative in large complex 
applications with many integrations.


If these controls are not possible considering using virtual patching, API security gateways, or web application firewalls (WAFs)
to detect, monitor, and block XXE attacks.
