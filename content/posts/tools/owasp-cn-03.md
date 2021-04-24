---
title: "OWASP 十大网站安全风险 （三）：敏感信息泄漏"
date: 2021-04-14
hero: /images/posts/pg.svg
menu:
    sidebar:
        name: owasp-3-sensitive-data-exposure
        identifier: owasp-3
        parent: tools
        weight: 10
---

## OWASP 十大信息安全主题
> 1. 注入攻击 （Injection）
> 2. 无效身份认证（Broken Authentication
> 3. **敏感信息泄漏（Sensitive Data Exposure）**
> 4. XML外部处理器漏洞（XML External Entities (XXE)）
> 5. 无效存取控管（Broken Access Control）
> 6. 错误设置安全系统（Security Misconfiguration）
> 7. 跨站攻击（Cross-Site Scripting (XSS)）
> 8. 不安全的反序列化漏洞（Insecure Deserialization）
> 9. 使用已有漏洞元件（Using Components with Known Vulnerabilities）
> 10. 日志和监控不足风险（Insufficient Logging and Monitoring）


## 敏感信息泄漏
Many web applications and APIs do not properly protect sensitive data, such as financial, healthcare,
and PII. Attackers may steal or modify such weakly protected data to conduct credit card fraud, identity
theft, or other crimes. Sensitive data may be compromised without extra protection, such as
encryption at rest or in transit, and requires special precautions when exchanged with the browser.

## 了解敏感信息泄漏
Sensitive data pii personally identifiable information,  PHI personal health information and an entire gamut of proprietary data

must be protected in all applications.


What do you need to know?

Consider who can gain access to your sensitive data, and any backups of that data. This includes the data at rest,
in transit, and even in your customers browsers.  Include both external and internal threats.


Rather than directly attacking crypto, attackers steel keys execute man-in-the-middle attacks or steel clear text data
off the server while in transit or from the users client, like a browser.


A manual attack is generally required. Previously retrieved password database, this could be forced by graphics processing units, gpus.


Over the last few years, this has been the most common impactful attack.

The most common flaw is simply not encrypting sensitive data. When crypto is employed, week key generation and management,
as well as weak algorithm, protocol ,and cyber usage is common, particularly for weak password hashing storage techniques.

For data in transit, server side weaknesses are mainly easy to detect, but hard for data at rest.


Failure frequently compromises all data that should have been protected. Typically, this information, including sensitive
personally identifiable information data (PPI), such as health records, credentials, personal data, and credit cards,
which often require protection as defined by laws or regulations, such as the EU GDPR or local privacy laws.


Consider the business value of the lost data and the impact or possible damage to your reputation.

What is your legal liability that this data is exposed?


The first thing you have to determine is which data is sensitive enough to require extra protection both in transit and at rest.


For examples, passwords, credit card numbers, health records, personal information, and business secrets require extra protection.


Particularly if that data falls under privacy laws or regulations, like EU's general data protection regulation gdpr, or
regulations for financial data protection, such as pci data security standard (pci DSS).


For all such data, is any data transmitted in clear text, this concerns protocols, such as http, SMTP, and ftp.

External internet traffic is especially dangerous, verifying all internal traffic between load balancers, web servers or back end systems.


Is sensitive data stored in clear text, including backups?
Are any old or weak cryptographic algorithms use either by default or in older code?

Are default crypto keys in use, weak crypto keys generated or re-used, or is proper key management for rotation missing?

Is encryption not enforced? Are any user agent browser security directives or headers missing?

Does user agent(like app, mail client) not verify if the received server certificate is valid?

Is the data included as plain text and application or server logs?

## 案例

##### scenario one

An application encrypts credit card numbers in a database using automatic database encryption. However, this data is
automatically decrypted when retrieved allowing a sequel injection flaw can retrieve credit card numbers in clear text.


##### scenario two
the site doesn't use or enforce TLS for all pages or supports weak encryption. An attacker monitors network
traffic(as at an insecure wireless network) downgrades connections from https to http, intercepts requests,
and steals the users' session cookie.


The attacker then replays this cookie and hijacks the users authenticated session, accessing or modifying the users' private data.
Instead of above, they could alter all transported data, like the recipient of a money transfer

##### scenario three.

The password database and uses unsalted or simple hashes to store everyone's passwords. A file upload flaw allows an attacker to
retrieve the password database.  All the unsalted hashes can be exposed with the Rainbow table
of pre-calculated hashes.  Hashes generated by simple or fast hash functions may be crackered by GPUs, even if they were
salted.


## 预防
all sensitive data to all of the following and then.


1. classify data processed, stored,  or transmitted by an application. Identify which data is sensitive according to privacy
   laws, regulatory requirements, or business needs.


2. Apply controls as proposed application.
   Don't store sensitive data unnecessarily. Discard it as soon as possible or use
   PCI DSS compliant tokenization or even truncation.  Data that is not retained cannot be stolen.


3. Make sure to encrypt all sensitive data at rest.  Ensure up-to-date and strong standard algorithms, protocols, and keys are
   in place; use property key management.


4. Encrypt all data in transit with secure protocols, such as TLS with perfect forward secrecy(PFS) ciphers, cipher prioritization
   by server, and secure parameters.  Enforce encryption using directives like http strict transport security(HSTS).


5. disable caching for responses that contain sensitive data.  
   Store passwords using a strong adaptive and salted hashing function with a work factor(delay factor), such as Argon2, scrypt,
   bcrypt, or PBKDF2.

6. Verify independently the effectiveness of configuration and settings.