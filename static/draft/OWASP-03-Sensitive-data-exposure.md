## Sensitive Data Exposure
Many web applications and APIs do not properly protect sensitive data, such as financial, healthcare,
adn PII. Attackers may steal or modify such weakly protected data to conduct credit card fraud, identity
theft, or other crimes. Sensitive data may be compromised without extra protection, such as
encryption at rest or in transit, and requires special precautions when exchanged with the browser.


### What should you know about 
Sensitive data pii personally identifiable information pHI personal health information and an entire gamut of proprietary data 

must be protected in all applications.


What do you need to know consider who can gain access to your sensitive data, and any backups of that data.


This includes the data at rest in transit, and even in your customers browsers include both external and internal threats.


Rather than directly attacking crypto attackers steel keys execute man in the middle attacks or steel clear text data 
off the server while in transit or from the users client like a browser.


A manual attack is generally required previously retrieved password database, this could be forced by graphics processing units gpus.


Over the last few years, this has been the most common impactful attack the most common law is simply not encrypting sensitive data.


And crypto is employed week key generation and management, as well as weak algorithm protocol and cyber usage is common, 
particularly for weak password hashing storage techniques for data in transit server side weaknesses are mainly easy to 
detect when hard for data at rest.


Failure frequently compromises all data that should have been protected.


Typically, this information, including sensitive personally identifiable information data API such as health records 
credentials personal data and credit cards, which often require protection as defined by laws or regulations, 
such as the EU gdpr or local privacy laws.


Consider the business value of the last day that and the impact or possible damage to your reputation, what is your 
legal liability that this data is exposed.


The first thing you have to determine is which data is sensitive enough to require extra protection, both in transit and at rest.


passwords credit card numbers, both records personal information and business secrets require extra protection.


Particularly if that data falls under privacy laws or regulations like he use general data protection regulation gdpr or 
regulations for financial data protection, such as pci data security standard pci DSS.


For all such data is any data transmitted in clear text, this concerns protocols, such as http s ftp ftp external 
Internet traffic is especially dangerous verify all internal traffic between load balancers, web servers or back end systems.


Sensitive data stored in clear text, including backups are any old are weak cryptographic algorithms use either by default or in older code.


or default crypto keys and use me crypto keys generated or reviews, for his property management for rotation missing is encryption not enforced.


Are any user agent browser security directives for headers and it doesn't user agent like at mail client not verify if the 
receipt server certificate is valid is the data included as plain text and application or server logs.

### Examples

##### scenario one 

an application includes credit card numbers and database using automatic database encryption.

However, this data is automatically decrypted when retrieved allowing a sequel injection law can retrieve credit card numbers, in clear text.


##### scenario two
the site doesn't use or enforce pls for all pages or supports encryption and attacker monitors network 
traffic as at an insecure wireless network.

downgrades connections from https or http intersects requests and steal to users session produce.


The attacker then replace this cookie and hijacks the users authenticated session accessing a lot of times when users private data.


Instead of about they could alter all transfer the data, like the recipient of the money transfer 

##### scenario three.

The password database and uses unsalted or simple caches to store everyone's passwords upload an attacker to 
retrieve the password database all the unsalted passions can be exposed for the Rainbow table 
at pre calculated hashes hashes generated more simple for fast.

track.


If they were assaulted.

### how to prevent
Girls unsafe cryptography ssl usage and data protection or well beyond the scope of the top 10 That said, for all 
sensitive data to all of the following and then.


classify data process storage or transmitted by an application identify which data is sensitive, according to privacy 
laws regulatory requirements or business needs.


apply controls as proposed application don't store sensitive data unnecessarily discarded as soon as possible for us 
pci DSS compliant organization or even truncation data that is not retained cannot be stolen.


Make sure to encrypt all sensitive data at rest ensure up today and strong standard algorithms protocols and keys are 
in place your property management.


encrypt all data in transit, the secure protocols, such as pls was perfect forward secrecy snipers cyber prioritization 
by server and secure parameters enforce encryption using directives like http strict transport security features to.


disable caching for responses that contain sensitive data store passwords using a strong adaptive and salted hashing 
function, so the work factor didn't like that So these are gone to script secret or pk do to verify independently 
effectiveness configurations and settings.
