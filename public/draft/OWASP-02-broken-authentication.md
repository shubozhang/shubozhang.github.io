# Broken Authentication
Application functions related to authentication and session management are often implemented incorrectly,
allowing attackers to compromise passwords, keys, or session tokens, or to exploit other implementation
flaws to assume other users' identities temporarily or permanently.

### What should you know about broken authentication

Attackers have access to hundreds of millions of valid username and password combinations for 
* credential stuffing 
  
* default administrator account list

* automated brute force 
  
* dictionary attack tools 
  
Session management attacks are well understood, particularly in relation to unexpired session tokens.

The prevalence of broken authentication is widespread due to the design and implementation of most identity and access controls.


Session management is the bedrock of authentication and access, controls and is present in all stateful applications.


Attackers can detect broken authentication using manual means, and exploit them using automated tools with password lists and dictionary attacks.


Attackers have to gain access to only a few accounts or just one admin account to compromise the system, depending on the domain of the
application, this may allow 

* money laundering 
  
* social security fraud 
  
* identity theft 
  
* disclosed legally protected highly sensitive information.

### Is the Application Vulnerable?

Confirmation of users identity, authentication ,and session management are critical to protect against authentication related attacks.

There may be authentication weaknesses if the application

* Permits automated attacks such as credential stuffing where the attacker has a list of valid usernames and passwords.

* Permits brute force or other automated attacks.
  
* Permits default, week, or well-known passwords such as "Password1" or "admin/admin".

* Uses week or ineffective credential recovery and forgot-password processes, such as knowledge-base answers which cannot be made safe.

* Uses plain text, encrypted, or weekly hashed passwords.

* Has missing or ineffective multi-factor authentication
  
* Exposes session IDs in the URL (for example, URL rewriting)

* Does not rotate session IDs after successful login
  
* does not properly invalidate session ids. User sessions or authentication tokens particularly single sign on sso tokens 
  aren't properly invalidated during logout, or a period of inactivity.

### Examples
##### scenario one 

credential stuffing , the use of lists of known passwords, is a common attack is an application does not
implement automated threat or credential stuffing protections,
the application can be used as a password Oracle to determine if the credentials are valid. 


##### scenario two

Most authentication attacks occur due to the continued use of passwords as a sole factor.
Once considered best practices, password rotation and complexity requirements are viewed as encouraging users to use,
and reuse, weak passwords. It is recommended that organizations stop these practices per NIST 800-63 and use multi-factor authentication 


##### scenario three.
Application session timeouts aren't set properly. A user uses a public computer to access an application, instead of selecting
logout user simply closes the browser tab and walks away and attackers uses the same browser an hour later, and the user is 
still authenticated.


### how to prevent it
* Where possible, implement multi-factor authentication to prevent

    * automated 
      
    * credential stuffing 
      
    * brute force 
      
    * stolen credential reuse attacks.


*Do not ship or deploy with any default credentials, particularly for admin users.


* Implement weak password checks, such as testing new or change passwords against a list of the top 10,000 worst passwords.


* Align password length, complexity and rotation policies with NIST 800-63 guidelines in section 5.1.1 for 
memorized secrets or other modern, evidence based password policies.


* Ensure registration, credential recovery, and API pathways are hardened against account Enumeration attacks by 
using the same messages for all outcomes.


* limit for increasingly delay failed login attempts log on failures and alert administrators from credential 
stuffing brute force or other attacks are detected.


* uses server side secure built in session manager that generates a new random session ID with high entropy after 
logging. session ID should not be in the URL be securely stored and invalidated after logout, idle ,and absolute timeouts.

