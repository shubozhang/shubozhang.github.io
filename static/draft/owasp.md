# OWASP 10

## Injection
## Broken Authentication
## Sensitive Data Exposure
## XML External Entities (XXE)
## Broken Access Control
## Security Misconfiguration
## Cross-Site Scripting (XSS)
## Insecure Deserialization
## Using Components with Known Vulnerabilities
## Insufficient Logging and Monitoring



1
00:00:18.840 --> 00:00:28.710
Shubo Zhang: We will follow the same general approach for each of the top 10 items first an overview description and graphics Second, we will look at how to judge vulnerability.

2
00:00:29.280 --> 00:00:45.120
Shubo Zhang: Third, will present a couple of attack scenarios which leads us to the port prevention approaches finally you'll find a comprehensive set of resources in the resources tab for your unlimited access use and distribution with that let's get started.

3
00:00:50.070 --> 00:01:05.280
Shubo Zhang: What should you know about injection consider anyone who can send untrusted data to the system, including external users internal users and administrators injection laws occur when an attacker sense hospital data to interpretive.

4
00:01:06.330 --> 00:01:12.750
Shubo Zhang: injection laws are very problem, particularly in legacy code injection vulnerabilities are all.

5
00:01:13.560 --> 00:01:26.250
Shubo Zhang: sequel l that X pack or no sequel queries os commands xml partners as mtp headers expression languages and rm queries.

6
00:01:26.730 --> 00:01:34.110
Shubo Zhang: injection flaws are easy to discover when examining code scanners and buzzers can help attackers find injection laws.

7
00:01:34.710 --> 00:01:51.810
Shubo Zhang: injection can result in data loss direction or disclosure to unauthorized parties loss of accountability or denial of access injection can sometimes lead to complete post take over the business impact depends on the needs of the application and data.

8
00:02:01.140 --> 00:02:10.320
Shubo Zhang: An application is vulnerable to injection attack when user supply data is not validated filter or sanitized by the application.

9
00:02:11.130 --> 00:02:18.660
Shubo Zhang: Dynamic queries or non parameter arrived calls without context aware escaping or used directly in the interpretive.

10
00:02:19.590 --> 00:02:41.490
Shubo Zhang: Hostile data issues with an object relational mapping rm search parameters to extract additional sensitive records hostile data is directly used or concatenate it such that the sequel or command contains both structure and hospital data in dynamic queries commands or stored procedures.

11
00:02:42.510 --> 00:02:51.180
Shubo Zhang: Some of the more common injections or sequel no sequel ios command object relational mapping rm fell down.

12
00:02:51.720 --> 00:02:58.230
Shubo Zhang: And expression language El or object graph navigation library oh gee NL injection.

13
00:02:58.890 --> 00:03:01.950
Shubo Zhang: The concept is identical among call interpreters.

14
00:03:02.310 --> 00:03:18.000
Shubo Zhang: source code review is the best method of detecting if applications are vulnerable to injections closely followed by thorough automated testing about parameters enters URL cookies Jason so an xml data inputs.

15
00:03:18.420 --> 00:03:33.420
Shubo Zhang: organizations can include static source SAS and dynamic application test test tools into the ci CD pipeline to identify newly introduced rejection was prior to production deployment.

16
00:03:41.190 --> 00:03:48.840
Shubo Zhang: scenario one the application uses untrusted data in the construction of the following vulnerable sequel call.

17
00:03:52.200 --> 00:04:05.220
Shubo Zhang: scenario to similarly and applications blind trust and frameworks may result in queries that are still vulnerable, for example, hibernate query language well.

18
00:04:10.170 --> 00:04:21.330
Shubo Zhang: This changes the meaning of both queries to return all the records from the accounts table more dangerous attacks could modify data or even invoke stored procedures.

19
00:04:31.980 --> 00:04:40.080
Shubo Zhang: injection requires keeping untrusted data, separate from cummings and queries the preferred option is to use the same API.

20
00:04:40.680 --> 00:04:45.960
Shubo Zhang: which have already through use of the interpreter entirely or provides a parameter is interprets.

21
00:04:46.740 --> 00:04:53.610
Shubo Zhang: Be careful with API such as stored procedures that are around your eyes, but can still introduce injection other would.

22
00:04:54.510 --> 00:05:07.110
Shubo Zhang: It would parameters API is not available, you should carefully escape special characters using the specific escape syntax for that interpretive oh last exactly provides many of these.

23
00:05:09.210 --> 00:05:18.030
Shubo Zhang: positive input validation is also recommended but it's not a complete Defense as many applications require special characters in their input.

24
00:05:19.290 --> 00:05:27.720
Shubo Zhang: You can also use women and other sequel controls within queries to prevent mass disclosure of records in case of sql injection.

25
00:05:36.450 --> 00:05:48.930
Shubo Zhang: What should you know about broken authentication attackers have access to hundreds of millions of valid username and password combinations for credential stuffing default administrator account list.

26
00:05:49.500 --> 00:05:55.740
Shubo Zhang: automated brute force and dictionary attack tools session management attacks and well understood.

27
00:05:56.910 --> 00:06:08.640
Shubo Zhang: In relation to unexpired session tokens the prevalence of authentication is widespread due to the design and implementation of most identity and access, controls.

28
00:06:09.390 --> 00:06:16.470
Shubo Zhang: session management is the bedrock of authentication and access, controls and is present in all staple applications.

29
00:06:16.890 --> 00:06:26.460
Shubo Zhang: attackers can detect broken authentication using manual means and it's going to use an automated tools, the password lists and dictionary attacks.

30
00:06:27.210 --> 00:06:46.440
Shubo Zhang: attackers have to gain access to only a few accounts or just one admin account to compromise the system, depending on the domain of the application, this may allow money laundering social security fraud and identity theft or disclosed legally protected highly sensitive information.

31
00:07:00.060 --> 00:07:08.340
Shubo Zhang: Confirmation of users identity authentication and session management are critical to protect against authentication related attacks.

32
00:07:09.060 --> 00:07:19.290
Shubo Zhang: And maybe authentication weaknesses, if the application from its automated attacks such as credential stuffing where the attacker has a list of valid usernames and passwords.

33
00:07:19.800 --> 00:07:29.880
Shubo Zhang: From its brute force or other automated attacks from its default week or well known passwords such as password one for admin admin.

34
00:07:31.260 --> 00:07:39.720
Shubo Zhang: uses week or ineffective credential recovery and provide password processes such as knowledge base answers which cannot be made safe.

35
00:07:40.830 --> 00:07:44.850
Shubo Zhang: uses plain text encrypted or weekly hashed passwords.

36
00:07:45.960 --> 00:07:55.050
Shubo Zhang: Has missing or ineffective multi factor authentication exposes session ids and the URL, for example, URL rewriting.

37
00:07:56.640 --> 00:08:14.760
Shubo Zhang: Does not rotate session ids after successful login does not properly invalidate session ids user sessions or authentication tokens particularly single sign on sso tokens aren't properly validated during logout or a period of inactivity.

38
00:08:23.880 --> 00:08:34.920
Shubo Zhang: scenario one credential stuffing the use of lists of known passwords is a common attack is an application does not implement automated threat or credentials.

39
00:08:35.340 --> 00:08:43.290
Shubo Zhang: protections the application can be used as a password Oracle to determine if the credentials about scenario to.

40
00:08:43.980 --> 00:08:49.500
Shubo Zhang: Most authentication attacks occur due to the continued use of passwords as a sole factor.

41
00:08:50.130 --> 00:08:59.550
Shubo Zhang: is considered best practices password rotation and complexity requirements are viewed as encouraging users to use and reuse passwords.

42
00:09:00.150 --> 00:09:10.440
Shubo Zhang: It is recommended that organizations stop these practices for nist 863 and use multi factor authentication scenario three.

43
00:09:11.220 --> 00:09:29.490
Shubo Zhang: Application session timeouts aren't set properly a user uses a public computer to access an application, instead of selecting logout user simply closes the browser tab and walks away and attackers uses the same browser hours later, and the user is still authenticated.

44
00:10:04.170 --> 00:10:13.320
Shubo Zhang: Where possible, implement multi factor authentication to prevent automated credential stuffing brute force and stolen credential reuse attacks.

45
00:10:14.370 --> 00:10:19.860
Shubo Zhang: Do not ship or deploy with any default credentials, particularly for admin users.

46
00:10:20.970 --> 00:10:29.250
Shubo Zhang: Implement weak password checks, such as testing or change passwords you can still list of the top 10,000 worst passwords.

47
00:10:30.270 --> 00:10:44.340
Shubo Zhang: align password length complexity and rotation policies with nist 863 D guidelines in section 5.1 point one for memorized secrets or other modern evidence based password policies.

48
00:10:45.180 --> 00:10:54.930
Shubo Zhang: Ensure registration credential recovery and API properties are hardened against account immigration attacks by using the same messages for all outcomes.

49
00:10:55.800 --> 00:11:06.180
Shubo Zhang: limit for increasingly delay failed login attempts log on failures and alert administrators from credential stuffing brute force or other attacks are detected.

50
00:11:06.960 --> 00:11:23.760
Shubo Zhang: he's a server side secure built in session manager that generates a new random session ID with high entropy after logging session ID should not be in the URL be securely stored and invalidated after logout idle an absolute times.

51
00:15:58.140 --> 00:16:10.590
Shubo Zhang: Sensitive data pii personally identifiable information pH I personal health information and an entire gamut of proprietary data must be protected in all applications.

52
00:16:11.250 --> 00:16:17.880
Shubo Zhang: What do you need to know consider who can gain access to your sensitive data, and any backups of that data.

53
00:16:18.630 --> 00:16:26.460
Shubo Zhang: This includes the data at rest in transit, and even in your customers browsers include both external and internal threats.

54
00:16:27.030 --> 00:16:40.350
Shubo Zhang: Rather than directly attacking crypto attackers steel keys execute man in the middle attacks or steel clear text data off the server while in transit or from the users client like a browser.

55
00:16:40.890 --> 00:16:50.370
Shubo Zhang: A manual attack is generally required previously retrieved password database, this could be forced by graphics processing units gpus.

56
00:16:51.150 --> 00:16:59.250
Shubo Zhang: Over the last few years, this has been the most common impactful attack the most common law is simply not encrypting sensitive data.

57
00:16:59.760 --> 00:17:17.730
Shubo Zhang: And crypto is employed week key generation and management, as well as weak algorithm protocol and cyber usage is common, particularly for weak password hashing storage techniques for data in transit server side weaknesses are mainly easy to detect when hard for data at rest.

58
00:17:18.780 --> 00:17:22.230
Shubo Zhang: Failure frequently compromises all data that should have been protected.

59
00:17:22.800 --> 00:17:41.010
Shubo Zhang: Typically, this information, including sensitive personally identifiable information data API such as health records credentials personal data and credit cards, which often require protection as defined by laws or regulations, such as the EU gdpr or local privacy laws.

60
00:17:41.940 --> 00:17:50.940
Shubo Zhang: Consider the business value of the last day that and the impact or possible damage to your reputation, what is your legal liability that this data is exposed.

61
00:17:57.690 --> 00:18:04.890
Shubo Zhang: The first thing you have to determine is which data is sensitive enough to require extra protection, both in transit and at rest.

62
00:18:06.090 --> 00:18:13.950
Shubo Zhang: passwords credit card numbers, both records personal information and business secrets require extra protection.

63
00:18:14.190 --> 00:18:30.240
Shubo Zhang: Particularly if that data falls under privacy laws or regulations like he use general data protection regulation gdpr or regulations for financial data protection, such as pci data security standard pci DSS.

64
00:18:31.320 --> 00:18:51.060
Shubo Zhang: For all such data is any data transmitted in clear text, this concerns protocols, such as http s ftp ftp external Internet traffic is especially dangerous verify all internal traffic between load balancers web servers or back end systems.

65
00:18:52.140 --> 00:19:02.130
Shubo Zhang: Sensitive data stored in clear text, including backups are any old are weak cryptographic algorithms use either by default or in older code.

66
00:19:03.120 --> 00:19:13.950
Shubo Zhang: or default crypto keys and use me crypto keys generated or reviews, for his property management for rotation missing is encryption not enforced.

67
00:19:14.640 --> 00:19:31.680
Shubo Zhang: Are any user agent browser security directives for headers and it doesn't user agent like at mail client not verify if the receipt server certificate is valid is the data included as plain text and application or server logs.

68
00:19:45.600 --> 00:19:53.370
Shubo Zhang: scenario one an application includes credit card numbers and database using automatic database encryption.

69
00:19:53.670 --> 00:20:03.090
Shubo Zhang: However, this data is automatically decrypted when retrieved allowing a sequel injection law can retrieve credit card numbers, in clear text.

70
00:20:03.990 --> 00:20:16.650
Shubo Zhang: scenario to the site doesn't use or enforce pls for all pages or supports encryption and attacker monitors network traffic as at an insecure wireless network.

71
00:20:17.070 --> 00:20:25.500
Shubo Zhang: downgrades connections from https or http intersects requests and steal to users session produce.

72
00:20:25.860 --> 00:20:34.350
Shubo Zhang: The attacker then replace this cookie and hijacks the users authenticated session accessing a lot of times when users private data.

73
00:20:34.920 --> 00:20:42.510
Shubo Zhang: Instead of about they could alter all transfer the data, like the recipient of the money transfer scenario three.

74
00:20:43.350 --> 00:21:03.030
Shubo Zhang: The password database and uses unsalted or simple caches to store everyone's passwords upload an attacker to retrieve the password database all the unsalted passions can be exposed for the Rainbow table at pre calculated ashes ashes generated more simple for fast.

75
00:21:04.290 --> 00:21:04.620
Shubo Zhang: track.

76
00:21:06.450 --> 00:21:07.470
Shubo Zhang: If they were assaulted.

77
00:21:15.240 --> 00:21:27.180
Shubo Zhang: Girls unsafe cryptography ssl usage and data protection or well beyond the scope of the top 10 That said, for all sensitive data to all of the following and then.

78
00:21:28.560 --> 00:21:38.640
Shubo Zhang: classify data process storage or transmitted by an application identify which data is sensitive, according to privacy laws regulatory requirements or business needs.

79
00:21:39.270 --> 00:21:54.870
Shubo Zhang: apply controls as proposed application don't store sensitive data unnecessarily discarded as soon as possible for us pci DSS compliant organization or even truncation data that is not retained cannot be stolen.

80
00:21:55.920 --> 00:22:06.810
Shubo Zhang: Make sure to encrypt all sensitive data at rest ensure up today and strong standard algorithms protocols and keys are in place your property management.

81
00:22:08.490 --> 00:22:26.400
Shubo Zhang: encrypt all data in transit, the secure protocols, such as pls was perfect forward secrecy snipers cyber prioritization by server and secure parameters enforce encryption using directives like http strict transport security features to.

82
00:22:27.870 --> 00:22:48.870
Shubo Zhang: disable caching for responses that contain sensitive data store passwords using a strong adaptive and salted hashing function, so the work factor didn't like that So these are gone to script secret or pk do to verify independently effectiveness configurations and settings.

83
00:22:57.180 --> 00:23:01.140
Shubo Zhang: What should you know about xml external entities or SMC.

84
00:23:01.860 --> 00:23:14.190
Shubo Zhang: attackers can exploit vulnerable xml processors if they can upload xml or include hostile content in an xml document exploiting vulnerable code dependencies or integrations.

85
00:23:14.910 --> 00:23:25.440
Shubo Zhang: by default many older xml processors a while specifications of an external entity, a you are I, that is the reference and evaluated during xml processing.

86
00:23:26.160 --> 00:23:44.850
Shubo Zhang: that's tools can discover this issue by inspecting dependencies and configuration dash tools require additional manual steps to the technical support position manual testers need to be trained in how to test for X sexy, as it is not commonly tested as of 2017.

87
00:23:45.900 --> 00:24:03.720
Shubo Zhang: These flaws, can be used to extract data executed remote request from the server scan internal systems and or perform a denial of service attack, as well as executed other attacks the business impact depends on the protections of all affected applications and data.

88
00:24:09.510 --> 00:24:31.170
Shubo Zhang: applications, and in particular xml based web services for downstream integrations might be vulnerable to attack is the application, except xml directly for xml uploads, especially from untrusted sources for inserts untrusted data into xml documents which is then parsed by an xml processor.

89
00:24:32.340 --> 00:24:41.820
Shubo Zhang: Any of the xml processors in the application or soap based web services has DVDs or document type definitions enable.

90
00:24:42.570 --> 00:24:54.450
Shubo Zhang: As the exact mechanism for disabled, the TV processing varies by processor, it is good practice to consult a reference, such as the old boss cheat sheet XX see prevention.

91
00:24:55.380 --> 00:25:08.850
Shubo Zhang: If you're African application was assembled for identity processing within federated security for single sign on sso purposes saml uses xml for identity assertions and maybe vulnerable.

92
00:25:09.900 --> 00:25:20.550
Shubo Zhang: If the application uses so prior to version 1.2 it is likely susceptible to XX he attacks the xml entity so being passed through so framework.

93
00:25:21.570 --> 00:25:31.200
Shubo Zhang: be vulnerable to accept the attacks likely means that the application is vulnerable to denial of service attacks, including 2 billion laughs attack.

94
00:25:39.300 --> 00:25:55.710
Shubo Zhang: Numerous public execs the issues have been discovered instantly attacking and then devices and sexy occurs in a lot of unexpected places, including deeply nested dependencies easiest way is to upload malicious xml file accepted.

95
00:25:59.250 --> 00:26:18.990
Shubo Zhang: scenario one the attacker attempts to extract data from the server scenario to an attack improves the servers private network by changing the below entity on scenario three an attacker attempts a denial of service attack by including a potentially endless file.

96
00:26:24.690 --> 00:26:28.170
Shubo Zhang: developer training is essential to identify and mitigate.

97
00:26:29.580 --> 00:26:41.490
Shubo Zhang: Besides that prevented sexy requires that you whenever possible use less complex data formats, such as Jason and avoid serialization of sensitive data.

98
00:26:42.450 --> 00:26:56.160
Shubo Zhang: path to upgrade all xml processors and libraries in use by the application or on the underlying operating system use dependency checkers update so just so 1.2 or higher.

99
00:26:57.210 --> 00:27:20.400
Shubo Zhang: disable xml external entity NDTV processing in all xml horses in the application as per the old boss chichi accepts the prevention implement positive server side input validation filtering or standardization, to prevent possible data within xml documents headers or nodes.

100
00:27:21.930 --> 00:27:31.710
Shubo Zhang: verify that xml or xml file upload functionality validates incoming xml using excess do validation or similar.

101
00:27:33.360 --> 00:27:44.730
Shubo Zhang: SAS tools can help detect Excellency and source code, although manual code review is the best alternative in large complex applications with many integrations.

102
00:27:45.180 --> 00:28:00.030
Shubo Zhang: If these controls are not possible considering using virtual patching API security gateways for web application firewalls wax to detect monitor and block exits he attacks.

103
00:28:08.220 --> 00:28:11.430
Shubo Zhang: What do you need to know about broken access control.

104
00:28:12.540 --> 00:28:24.810
Shubo Zhang: Exploitation of access control is a core skill of attackers SAS and best tools can detect the absence of access control, but cannot verify if it is functional when it is present.

105
00:28:25.680 --> 00:28:34.740
Shubo Zhang: Access controls detectable user manual means or possibly through automation for the absence of access, controls in certain frameworks.

106
00:28:36.060 --> 00:28:45.390
Shubo Zhang: access control weaknesses are common due to the lack of automated detection and lack of effective functional testing by application developers.

107
00:28:45.930 --> 00:28:52.020
Shubo Zhang: access control detection is not typically amenable to automated static or dynamic testing.

108
00:28:52.560 --> 00:29:05.790
Shubo Zhang: manual testing is the best way to detect missing or an effective access control, including http nothing yet versus current controller direct object references, etc.

109
00:29:06.780 --> 00:29:24.900
Shubo Zhang: The technical impact is attackers acting as users or administrators for users accessing privileged functions for creating accessing updated or deleting every record the business impact depends on the protection needs of the application and then.

110
00:29:31.770 --> 00:29:49.110
Shubo Zhang: access control and horses policy, such that users cannot act outside of the intended permissions they all yours typically lead to an authorized relations corrosion multiplication or destruction of data or performing the business function outside of the limits of the user.

111
00:29:50.310 --> 00:30:04.800
Shubo Zhang: Common access control Roman abilities include bypassing access control checks my mom on the URL internal application sleep for the html page or simply using a custom API attack to.

112
00:30:05.850 --> 00:30:12.750
Shubo Zhang: Allow the primary key to change to another user's record committing viewing or editing someone else's account.

113
00:30:14.100 --> 00:30:22.140
Shubo Zhang: elevation of privilege acting as a loser, without being logged in for acting as an admin when logged in as a loser.

114
00:30:23.610 --> 00:30:44.280
Shubo Zhang: Meta data manipulation, such as replay or tampering for the json web token jm T access control token or a cookie or cake you feel manipulated to elevate privileges for abusing J the beauty invalidation course miss configuration allows unauthorized API access.

115
00:30:45.540 --> 00:30:58.050
Shubo Zhang: or browsing to authenticated pages as an authenticated user for to privileged pages as a standard user accessing API missing access, controls for pumps.

116
00:31:13.980 --> 00:31:21.480
Shubo Zhang: scenario one the application uses unverified data in a sequel call that is accessing account information.

117
00:31:23.610 --> 00:31:35.670
Shubo Zhang: And attacker simply modifies the account parameter in the browser to send whatever account number, they want, if not properly verified the attacker can access any account.

118
00:31:37.110 --> 00:31:57.240
Shubo Zhang: scenario to an attacker simply forces browsers to target URLs admin rights are required for access to the admin page if an authenticated user can access either page it's a flaw if a non admin can access the admin page, this is a block.

119
00:32:06.330 --> 00:32:22.680
Shubo Zhang: access control is only effective they've been forced to trusted server side code or server list API or the attacker cannot modify the access control check or metadata there are some techniques, you can follow to prevent broken access control.

120
00:32:24.000 --> 00:32:27.540
Shubo Zhang: With the exception of public resources denied by default.

121
00:32:29.040 --> 00:32:37.080
Shubo Zhang: Implement access control mechanisms once and we use them throughout the application, including minimizing portion usage.

122
00:32:38.760 --> 00:32:48.030
Shubo Zhang: model access, controls should enforce record ownership, rather than accepting that the user can create read update or delete any record.

123
00:32:49.560 --> 00:32:54.660
Shubo Zhang: meet application business limit requirements should be enforced by domain models.

124
00:32:56.400 --> 00:33:05.880
Shubo Zhang: disable web server directory listing and ensure file metadata like dot govt and backup files are not present within one groups.

125
00:33:07.410 --> 00:33:13.500
Shubo Zhang: log access control failures alert admins who are the appropriate like repeated failures.

126
00:33:14.760 --> 00:33:20.700
Shubo Zhang: rate limit API and controller access to minimize the harm from automated attack tooling.

127
00:33:21.930 --> 00:33:26.610
Shubo Zhang: J wt tokens should be invalidated on the server after log APP.

128
00:33:28.080 --> 00:33:34.440
Shubo Zhang: developers and QA SAP should include functional access control unit and integration tests.

129
00:33:47.910 --> 00:34:06.120
Shubo Zhang: One of the concerns when programming applications to avoid security miss configuration attackers will attempt to exploit unpatched bars or access to bank accounts and used pages unprotected files and directories etc to gain unauthorized access or knowledge of the system.

130
00:34:07.260 --> 00:34:16.590
Shubo Zhang: Security miss configuration can happen at any level of any application stack, including the network services platform web server application server.

131
00:34:16.920 --> 00:34:34.290
Shubo Zhang: database frameworks custom code and pre installed virtual machines containers for storage automated scanner some useful for detecting this configurations use the default accounts for configurations unnecessary services like this the options, etc.

132
00:34:35.370 --> 00:34:50.400
Shubo Zhang: Such laws frequently give attackers unauthorized access to some system data or functionality occasionally such laws, result in a complete system compromise, but this is impacted depends on the protection needs of the application and data.

133
00:35:00.240 --> 00:35:11.670
Shubo Zhang: The application might be vulnerable if the application is this inappropriate security hardening across any part of the application stack or improperly configure permissions on cloud services.

134
00:35:12.540 --> 00:35:19.080
Shubo Zhang: Unnecessary features are enabled or install like unnecessary ports services pages accounts or.

135
00:35:20.610 --> 00:35:30.900
Shubo Zhang: default accounts and their passwords still enabled and unchanged error handling reveals stack traces or other overland informative error messages to users.

136
00:35:31.980 --> 00:35:37.590
Shubo Zhang: For upgraded systems, ladies security features are disabled or not configured security.

137
00:35:38.910 --> 00:36:01.110
Shubo Zhang: The security settings in the application servers application frameworks like struts spring Asp net libraries databases, etc, not set to secure values server does not send security headers and directives or they are not set to secure values software is out of date or vulnerable.

138
00:36:02.520 --> 00:36:09.240
Shubo Zhang: Without concerted repeatable application security configuration process systems are at a higher risk.

139
00:36:21.720 --> 00:36:34.860
Shubo Zhang: scenario one the application server comes good sample applications that are not removed from the production server the sample applications have known security dos attack refuse to compromise the server.

140
00:36:36.210 --> 00:36:44.610
Shubo Zhang: If one of these applications as the admin console and default accounts won't change the attacker logged in with default passwords and takes over.

141
00:36:45.660 --> 00:36:53.850
Shubo Zhang: scenario to directory listing is not disabled on the server and attacker to establish they consider list directories.

142
00:36:54.210 --> 00:37:06.120
Shubo Zhang: The attacker fines and downloads to compile Java classes, which they D compile and reverse engineer to view the Code, the attacker then finds a serious access control law in the application.

143
00:37:07.410 --> 00:37:25.050
Shubo Zhang: scenario three the application servers configuration allows detail error messages like stack traces to be returned to users this potentially exposes sensitive information nation or underlying laws such as component versions that are known to be vulnerable.

144
00:37:26.160 --> 00:37:39.120
Shubo Zhang: scenario for a phone service provider has default sharing permissions open to the Internet by other CSP users, this allows sensitive data stored within cloud storage to be accents.

145
00:37:44.100 --> 00:37:46.890
Shubo Zhang: Secure installation processes should be implemented.

146
00:37:49.290 --> 00:37:55.890
Shubo Zhang: A repeatable hardening process that makes it fast and easy to deploy another environment for this property lockdown.

147
00:37:56.640 --> 00:38:11.790
Shubo Zhang: Development QA and production environments should all be configured identically different credentials use each environment this process should be automated to minimize the effort required to set up a new secure environment.

148
00:38:14.040 --> 00:38:24.330
Shubo Zhang: The mobile platform, without any unnecessary features components documentation and samples they move or do not install unused features and framework.

149
00:38:27.060 --> 00:38:41.880
Shubo Zhang: A task to review and update the configurations appropriate all security knows updates and patches as part of the patch management process, in particular, who you called storage permissions like s3 bucket permissions.

150
00:38:44.400 --> 00:38:56.640
Shubo Zhang: A segmented application architecture that provides effective secure separation between components for tenants and segmentation containerization for cloud security groups.

151
00:38:58.920 --> 00:39:05.310
Shubo Zhang: An automated process to verify the effectiveness of the configurations and settings in all environments.

152
00:39:16.800 --> 00:39:18.750
Shubo Zhang: What should you know about prospects.





####  section 2
WEBVTT

1
00:00:19.980 --> 00:00:24.090
Shubo Zhang: Three four cents and there are freely available in.

2
00:00:25.650 --> 00:00:28.740
Shubo Zhang: existence of the second most prevalent issue in the lost.

3
00:00:29.760 --> 00:00:32.970
Shubo Zhang: and found the nearly two thirds of all applications.

4
00:00:34.260 --> 00:00:46.410
Shubo Zhang: automated tools and find some extra problems are manifest, particularly in mature technology such as php J to w gst and he got half.

5
00:00:47.190 --> 00:00:57.810
Shubo Zhang: Of the three forms of exercise the impact of existence is moderate for both reflection access and dawn excellent says, and the impact of severe for storage, etc.

6
00:00:58.830 --> 00:01:07.770
Shubo Zhang: These attacks can result in remote code execution on the victims browser such as potential sessions or delivery malware.

7
00:01:16.740 --> 00:01:30.630
Shubo Zhang: For three forms and access our browsers reflected access the application or API food some validate with an unstable user input, as part of html output.

8
00:01:31.200 --> 00:01:49.290
Shubo Zhang: A successful attack and allow the attacker to execute arbitrary html and javascript in the browser typically the user will need to interact with some malicious link that points to an attacker controlled page, such as malicious watering hole websites advertisement or something.

9
00:01:50.700 --> 00:01:53.220
Shubo Zhang: store in excess yeah.

10
00:01:55.350 --> 00:01:57.450
Shubo Zhang: And sanitize user in first.

11
00:01:58.770 --> 00:02:00.630
Shubo Zhang: Time user or at.

12
00:02:02.010 --> 00:02:03.240
School excellent second.

13
00:02:04.380 --> 00:02:04.740
Shubo Zhang: Time.

14
00:02:07.980 --> 00:02:13.470
Shubo Zhang: Access javascript frameworks single page applications API.

15
00:02:15.870 --> 00:02:27.480
Shubo Zhang: control the data to a page or vulnerable to Dom access so ideally the application is not a complete control data is a javascript API.

16
00:02:28.560 --> 00:02:34.680
Shubo Zhang: Typical access attacks session senior accountant over anything by.

17
00:02:35.730 --> 00:02:39.780
Shubo Zhang: Tom those will be facing such a stroke involving.

18
00:02:41.070 --> 00:02:48.270
Shubo Zhang: Accidents abuse or strauss's second solution malicious software downloads the logging and other client side attacks.

19
00:02:56.730 --> 00:03:04.050
Shubo Zhang: The application uses untrusted data and the construction of the following html snippet without validation for a statement.

20
00:03:07.410 --> 00:03:10.410
Shubo Zhang: This is a perfect example of code and data mixture.

21
00:03:13.650 --> 00:03:14.880
Shubo Zhang: In this case, the browser.

22
00:03:19.980 --> 00:03:26.550
Shubo Zhang: All data is treated as trusted this problem, second part of all input validation will choose.

23
00:03:28.200 --> 00:03:32.280
Shubo Zhang: The attacker modifies the system parameters and these grounds, actually.

24
00:03:35.670 --> 00:03:50.820
Shubo Zhang: This causes the victim session I accepted the attackers website allowing attacked by jack up to scorn session know attackers can also lose access to automotive CSR that becomes the application.

25
00:03:59.820 --> 00:04:07.680
Shubo Zhang: Access is required separation untrusted been impacted browser content can be achieved by doing all the follow.

26
00:04:08.730 --> 00:04:18.300
Shubo Zhang: These frameworks that automatically gets access card design, such as the latest Ruby on rails react to us working on the.

27
00:04:24.570 --> 00:04:24.930
Shubo Zhang: problem.

28
00:04:26.250 --> 00:04:33.450
Shubo Zhang: Statement contrasting http request, based on the context in which to outfit ADI.

29
00:04:34.800 --> 00:04:41.520
Shubo Zhang: javascript CSS for URL will resolve lucky and sort exercise for.

30
00:04:43.020 --> 00:04:49.350
Shubo Zhang: You oh was teaching exercise prevention as details on fire data saving techniques.

31
00:04:51.150 --> 00:05:12.180
Shubo Zhang: Some point context sensitive imploding and modifying the browser documents on the client side accidents, Dom access from this cannot be avoided similar context sensitive of saving time, like the browser api's as described or loss cheat sheet down based access as prevention.

32
00:05:14.130 --> 00:05:24.750
Shubo Zhang: enablement content security policy CSP prison Defense in depth mitigated control events XX XX it is affected, no other ones.

33
00:05:26.280 --> 00:05:28.380
Shubo Zhang: That will allow malicious code.

34
00:05:29.640 --> 00:05:33.270
Shubo Zhang: Like path to personal overrides or libraries.

35
00:06:03.510 --> 00:06:04.290
Shubo Zhang: On the show.

36
00:06:05.910 --> 00:06:08.400
Shubo Zhang: about changes and tweaks to the underlying.

37
00:06:10.530 --> 00:06:22.440
Shubo Zhang: issues in the top 10 based on day one day some tools can discover the civilization flaws in the system is frequently.

38
00:06:23.520 --> 00:06:33.330
Shubo Zhang: Because it's expected that prevalence data for civilization was will increase as Troy is developed and help identify and address it.

39
00:06:34.530 --> 00:06:37.980
Shubo Zhang: The impact to do serialization crossing state of it.

40
00:06:43.650 --> 00:06:44.130
Shubo Zhang: possible.

41
00:07:12.870 --> 00:07:14.760
Shubo Zhang: objects and find valuable time to.

42
00:07:15.870 --> 00:07:34.920
Shubo Zhang: resolve on their terms of the tax office and baby structured related attacks when we attempt to notify application walking or choose arbitrary remote code execution, there are classes available to the application that can change behavior during or after the civilization.

43
00:07:36.120 --> 00:07:40.860
Shubo Zhang: Typical data tampering attacks such as access control related to tax for.

44
00:07:42.360 --> 00:07:44.160
Shubo Zhang: US for the content was changed.

45
00:07:45.480 --> 00:07:53.400
Shubo Zhang: civilization some applications for we wrote into the process communication rpc I can see.

46
00:07:54.510 --> 00:08:04.470
Shubo Zhang: Why are protocols web services message burgers passion for systems databases cash service file systems.

47
00:08:05.550 --> 00:08:10.920
Shubo Zhang: http cookie html form parameters API authentication token.

48
00:08:25.980 --> 00:08:27.270
Shubo Zhang: react application.

49
00:08:36.420 --> 00:08:36.990
Shubo Zhang: solution.

50
00:08:38.190 --> 00:08:39.150
Shubo Zhang: Is serialized.

51
00:08:40.350 --> 00:08:50.190
Shubo Zhang: and passing back and forth with each request and attack for nurses, they are 00 Java objects and ensure and uses the Java Su.

52
00:08:51.330 --> 00:09:15.150
Shubo Zhang: To gain remote code execution on the application server into a php forum uses php object serialization to say a super cookie containing the users user ID role possible hash and other states and attacker changes the serialized object to give themselves admin privileges.

53
00:09:26.340 --> 00:09:37.710
Shubo Zhang: The only safe architectural pattern is not to accept some normalized objects from untrusted sources for to use serialization means that only primitive data types.

54
00:09:38.550 --> 00:09:51.300
Shubo Zhang: It sounds not possible see one or more of the following implementing integrity checks, such as digital signatures on n serialized objects to prevent possible object creation or data template.

55
00:09:52.470 --> 00:10:09.510
Shubo Zhang: Enforcing script type constraints during the serialization before entrepreneur nation as the code typically expect a definable set of crisis bypasses to this technique i've been demonstrating so alliance Simon on this is not advising.

56
00:10:10.680 --> 00:10:15.840
Shubo Zhang: Isolating and running code that be serialized a roku deployments when possible.

57
00:10:17.190 --> 00:10:26.700
Shubo Zhang: Logging the serialization exceptions and failures, such as for the incoming tide is not the expected type or the DC realization throws exception.

58
00:10:28.830 --> 00:10:35.910
Shubo Zhang: Restricting monitoring incoming and outgoing network connectivity containers or servers that you see remote.

59
00:10:37.950 --> 00:10:43.260
Shubo Zhang: Monitoring serialization a learning that we use in the serialized is constantly.

60
00:11:04.110 --> 00:11:05.040
Shubo Zhang: Using software.

61
00:11:06.780 --> 00:11:14.250
Shubo Zhang: Is captured to secure application develop what should we do to avoid it while he's already.

62
00:11:15.150 --> 00:11:22.020
Shubo Zhang: turned around vulnerabilities other vulnerable, which requires constant effort to develop a custom exploits.

63
00:11:22.920 --> 00:11:36.540
Shubo Zhang: prevalence of this issue is very widespread component heavy development patterns to need to development team it's, not even when you're standing which can save us in their application or API much less keeping them up today.

64
00:11:37.410 --> 00:11:45.150
Shubo Zhang: Some scanners so get tired js help in detection, but determining exploit ability requires additional.

65
00:11:47.520 --> 00:11:49.800
Shubo Zhang: vulnerabilities only binary.

66
00:12:40.830 --> 00:12:57.150
Shubo Zhang: Software is vulnerable unsupported or out of date, this includes the Ls web application certain database management system cms applications API and all components runtime environments and library.

67
00:12:58.500 --> 00:13:04.980
Shubo Zhang: If you do not scan for vulnerabilities and subscribe to security bulletins related to the moment to view.

68
00:13:06.390 --> 00:13:26.880
Shubo Zhang: If you cannot fix or upgrade the underlying platform frameworks and in this space timely fashion, this only happens environments and patching is a monthly or quarterly task and change control, which leads organizations, open to many days or months of unnecessary exposure to fix.

69
00:13:29.310 --> 00:13:34.800
Shubo Zhang: Their software developers do not test the compatibility of updated upgraded okay why.

70
00:13:36.750 --> 00:13:39.330
Shubo Zhang: not secure the components configurations.

71
00:13:47.460 --> 00:14:02.040
Shubo Zhang: components typically run with the same privileges as the application itself so balls in any component can result in serious and such laws can be accidental like coding error or intentional like back door and component.

72
00:14:03.270 --> 00:14:20.550
Shubo Zhang: Some example exploitable component vulnerabilities discovered our CV 2017 5638 a stretch to remote code execution vulnerability that enables execution of arbitrary code on the server has been blamed for significant breaches.

73
00:14:22.470 --> 00:14:32.310
Shubo Zhang: While Internet of Things iot are frequently difficult or impossible to patch the importance of patching and can be great such as biomedical devices.

74
00:14:33.420 --> 00:14:48.810
Shubo Zhang: There are automated tools to help attackers fine unpatched or miss configure systems, for example to show them iot search engine can help you find devices that still suffer from the heartbeat vulnerability patched in April 2014.

75
00:14:52.950 --> 00:14:57.960
Shubo Zhang: One option is not to use components that you didn't like but that's not very realistic.

76
00:14:58.770 --> 00:15:08.220
Shubo Zhang: There should be a patch management process in place to remove unused dependencies unnecessary features components files and documentation.

77
00:15:09.090 --> 00:15:22.620
Shubo Zhang: continuously inventory, the versions of both client side and server side components, such as frameworks and libraries and the dependencies using tools like versions dependency check retire decay, so instead of.

78
00:15:23.580 --> 00:15:39.750
Shubo Zhang: continuously monitor sources like see me and bb for vulnerabilities and the components you software composition analysis tools to automate the process subscribe to email alerts for security vulnerabilities related to conform to use.

79
00:15:41.070 --> 00:15:50.520
Shubo Zhang: Only attainable it's not official sources over secure link prefer side packages to reduce the chance of a modified malicious component.

80
00:15:51.570 --> 00:15:57.990
Shubo Zhang: monitor for libraries and components that are unmaintained or do not create security patches for older versions.

81
00:15:58.650 --> 00:16:06.630
Shubo Zhang: Because it's not possible consider deploy a virtual patch to launder detect or protect against the discovery issue.

82
00:16:07.500 --> 00:16:19.500
Shubo Zhang: Every organization must ensure that there is an ongoing plan for monitoring triage and and applying updates or configuration changes to the lifetime of the application or portfolio.

83
00:16:38.100 --> 00:16:47.190
Shubo Zhang: What should you know about insufficient logging and monitoring exploitation of insufficient logging and monitoring is the bedrock of nearly every major incident.

84
00:16:48.090 --> 00:16:57.360
Shubo Zhang: attackers rely on the lack of monitoring timely response to achieve their goals without being detected, this issue is included in the top 10 based on.

85
00:16:58.920 --> 00:17:01.740
Shubo Zhang: One strategy for determining if you have sufficient on.

86
00:17:03.510 --> 00:17:04.680
Shubo Zhang: phone penetration.

87
00:17:05.700 --> 00:17:15.180
Shubo Zhang: testers actions should be recorded sufficiently to understand images they may have incentive most successful attack start with vulnerability probing.

88
00:17:15.900 --> 00:17:29.850
Shubo Zhang: Allowing such probes to continue can raise the likelihood of success to you 100% in 2016 identifying the breach is an aggregate 191 days plenty of time for damage to be inflicted.

89
00:17:33.630 --> 00:17:45.120
Shubo Zhang: Insufficient logging detection monitoring and active response occurs anytime auditable events such as logins failed logins and high value transactions are not logged.

90
00:17:46.050 --> 00:17:55.650
Shubo Zhang: warnings and errors generate know inadequate or unclear block messages logs of applications and api's are not monitored for suspicious activity.

91
00:17:56.820 --> 00:17:58.680
Shubo Zhang: logs are only stored locally.

92
00:17:59.910 --> 00:18:05.520
Shubo Zhang: Appropriate alerting threshold and response escalation processes are not in place for effective.

93
00:18:06.810 --> 00:18:13.470
Shubo Zhang: penetration testing and stands by desks tools such as for was that do not trigger alerts.

94
00:18:14.520 --> 00:18:21.300
Shubo Zhang: The application is unable to detect escalate for alert for active attacks in real time or near real time.

95
00:18:22.890 --> 00:18:29.070
Shubo Zhang: You are vulnerable to information with like logging and learning events visible to the user or an attacker.

96
00:18:54.900 --> 00:19:01.290
Shubo Zhang: An open source forum software project run by a small team was hacked using a flaw and software.

97
00:19:01.770 --> 00:19:18.960
Shubo Zhang: The attackers managed to wipe out the internal source code repository containing the next version and all of the foreign contents all the source could be recovered lack of monitoring logging for alerting led to a far worse breach the forum software subject is no.

98
00:19:20.100 --> 00:19:20.490
Shubo Zhang: result.

99
00:19:22.440 --> 00:19:43.170
Shubo Zhang: scenario to an attacker uses scans for users, using the common password they can take over all accounts, using this password for all other users can leaves only one false login on Sunday may be repeated, with a different password scenario three a major US return.

100
00:19:44.370 --> 00:19:49.890
Shubo Zhang: Internal malware analysis sandbox analyzing attachments sandbox software.

101
00:19:51.720 --> 00:20:03.180
Shubo Zhang: Software so no one responded to this detection sandbox I did producing mornings, for some time before the breach was detected dude of fraudulent card transactions by an external bank.

102
00:20:07.680 --> 00:20:11.670
Shubo Zhang: As for the risk of the data is stored or processed by the application.

103
00:20:12.450 --> 00:20:25.740
Shubo Zhang: ensure all invalid login attempts access control failures and server side input validation failures can be logged in sufficient user context to identify suspicious or malicious accounts.

104
00:20:26.160 --> 00:20:43.830
Shubo Zhang: And held for sufficient time to allow delayed forensic analysis, ensure that logs generated in a format that can be easily consumed by centralized log management solutions ensure high value transactions have an audit trail with integrity controls tampering.

105
00:20:45.660 --> 00:20:46.890
Shubo Zhang: And only database.

106
00:20:50.220 --> 00:21:00.960
Shubo Zhang: Monitoring and alerting such that suspicious activities are detected and responded to in a timely fashion, establish or adopting incident response and recovery plan.

107
00:21:02.190 --> 00:21:07.260
Shubo Zhang: 851 read two or later there are commercial and open.

108
00:21:39.000 --> 00:21:43.110
Shubo Zhang: has for the risk of the data stored or process by the application.

109
00:21:43.890 --> 00:21:57.120
Shubo Zhang: ensure all invalid login attempts access control failures and precise input validation failures can be launched with sufficient user context to identify suspicious or malicious accounts.

110
00:21:57.570 --> 00:22:09.000
Shubo Zhang: And held for sufficient time to allow the raid forensic analysis to ensure that logs are generated in a format that can be easily consumed by centralized log management solutions.

111
00:22:09.480 --> 00:22:19.560
Shubo Zhang: ensure high value transactions have an audit trail with integrity controls to prevent tampering or deletion, such as append only database tables for similar.

112
00:22:20.550 --> 00:22:28.080
Shubo Zhang: establish effective monitoring and alerting such that suspicious activities are detected and responded to in a timely fashion.

113
00:22:29.040 --> 00:22:36.480
Shubo Zhang: Establish or adopting incident response and recovery planning, such as Miss 861 read two or later.

114
00:22:37.200 --> 00:22:55.260
Shubo Zhang: There are commercial and open source application protection frameworks, such as a was APP Center web application firewalls such as MOD security would be oh was Mon security poor rule set and blog correlation software with custom dashboards and alerting.

115
00:23:01.170 --> 00:23:12.540
Shubo Zhang: Whether you're new to web application security or are already very familiar with these risks fantastic producing a secure web application for fixing an existing one can be difficult.

116
00:23:13.020 --> 00:23:24.690
Shubo Zhang: If you have to manage a large application portfolio, this can be daunting to help organizations and developers reduce their application security or cost effective manner or wants.

117
00:23:28.770 --> 00:23:29.850
Shubo Zhang: To address application.

118
00:23:38.280 --> 00:23:40.530
Shubo Zhang: Secure web application, you must have.

119
00:23:42.060 --> 00:23:42.360
Shubo Zhang: Your.

120
00:23:43.500 --> 00:23:58.920
Shubo Zhang: Patience oh last recommend to use the ios application security verification standard Asp s as a guide for settings security requirements for your application you're outsourcing consider the last secure software contract and.

121
00:24:03.900 --> 00:24:17.730
Shubo Zhang: Rather than retrofitting security into your applications is far more cost effective to design in the security right from the start you've heard of the overwatch absence and project for details on the successful security architecture.

122
00:25:49.980 --> 00:25:58.290
Shubo Zhang: security controls is difficult, using a set of standard security controls radically simplifies the development of secure applications and.

123
00:25:59.700 --> 00:26:13.260
Shubo Zhang: The almost proactive goals is a good starting point for developers and many modern frameworks now come with standard and effective security controls for optimization validation csrs prevention it's endless.

124
00:26:19.680 --> 00:26:27.720
Shubo Zhang: process your organization follows when building such applications oh was recommends the software assurance maturity model.

125
00:26:29.160 --> 00:26:37.830
Shubo Zhang: This model organization and implement for software security tailored to specific risks at your organization.

126
00:26:43.230 --> 00:26:54.480
Shubo Zhang: We all want education project provides training materials to help educate developers on web application security and has been filed a large list of a was educational presentations.

127
00:26:54.960 --> 00:27:06.690
Shubo Zhang: For hands on learning about vulnerabilities try oh last web goat to stay current come to an O was upset conference oh was competence training for mobile a wasp chapter.

128
00:27:24.210 --> 00:27:35.400
Shubo Zhang: Now we secure code is important but it's critical to verify that the security you intended to build in is actually present correctly implemented and he's everywhere, it is supposed to be.

129
00:27:36.420 --> 00:27:39.600
Shubo Zhang: To go application security provide.

130
00:27:40.920 --> 00:27:51.060
Shubo Zhang: The work is difficult and complex and modern, high speed development processes like agile and devops have extreme pressure on traditional approaches and tools.

131
00:27:51.780 --> 00:28:07.500
Shubo Zhang: So we strongly encourage you to put some thought into how you're going to focus on what's important across your entire application portfolio and do it cost effectively modern risks, so the days of scanning for penetration testing application.

132
00:28:12.420 --> 00:28:29.070
Shubo Zhang: development requires you to use application security testing across the entire software development lifecycle look to enhance existing development pipelines with security automation that doesn't slow development whatever approach you choose consider the annual cost expressed.

133
00:28:30.240 --> 00:28:37.740
Shubo Zhang: me a retest and redeploy a simple application multiplied by the size of your application portfolio.

134
00:28:41.970 --> 00:28:52.200
Shubo Zhang: before you start testing be sure you understand what's important to spend time on priorities come from the threat model, so you don't have one, we need to create one before testing.

135
00:28:54.300 --> 00:29:01.620
Shubo Zhang: As vs and we are was testing guide as an input and don't rely on tool vendors, to decide what's important.

136
00:29:20.940 --> 00:29:22.290
Shubo Zhang: approach to applications.

137
00:29:23.550 --> 00:29:32.130
Shubo Zhang: must be highly compatible with the people processes and tools you use in your software development lifecycle as dlc.

138
00:29:32.760 --> 00:29:46.830
Shubo Zhang: Attempts to force extra steps gates and reviews are likely to cause friction get bypass and struggle to scale look for natural opportunities to gather security information and feed it back into your process.

139
00:29:51.690 --> 00:29:57.210
Shubo Zhang: choose the simplest fastest, most accurate technique to verify each require.

140
00:29:58.740 --> 00:30:03.000
Shubo Zhang: Security knowledge and oh was application security.

141
00:30:07.260 --> 00:30:07.830
Shubo Zhang: Security.

142
00:30:41.250 --> 00:30:57.690
Shubo Zhang: don't want to start out testing everything focus on what's important and expand your verification program for the time that means expanding the set of security defenses and risks that are being automatically there as well as expanding the set of applications and a.

143
00:31:00.600 --> 00:31:06.780
Shubo Zhang: State where the essential structure of your all your applications and API is continuously.

144
00:31:18.300 --> 00:31:20.160
Shubo Zhang: No matter how good you are testing.

145
00:31:21.900 --> 00:31:41.070
Shubo Zhang: She communicated effectively build trust by showing understand how the application works describe clearly how it can be used without lingo and include an attack scenario to make it real make a realistic estimation of how difficult for vulnerability is to discover and explore.

146
00:31:42.570 --> 00:31:48.450
Shubo Zhang: That would be finally deliver findings in the tools development teams are using.

147
00:31:55.260 --> 00:31:59.430
Shubo Zhang: Security is no longer function increasing attack.

148
00:32:02.130 --> 00:32:05.220
Shubo Zhang: must establish and effective capabilities for security, there are.

149
00:32:06.960 --> 00:32:13.830
Shubo Zhang: staggering number of applications and lines of code already in production, many organizations are struggling to get a handle.

150
00:32:17.100 --> 00:32:26.520
Shubo Zhang: On wasp recommends that organization application security program to gain insight and improve security across their application for.

151
00:32:29.070 --> 00:32:33.840
Shubo Zhang: security requires many different parts of an organization to work together efficiently.

152
00:32:36.390 --> 00:32:53.070
Shubo Zhang: software development and business and executive management, it requires security to be visible, so that all the different players see and understand the organization's application security posture it also requires focus on the activities and outcomes that actually no.

153
00:32:54.330 --> 00:32:57.960
Shubo Zhang: fury or reducing risk in the most cost effective manner.

154
00:33:03.840 --> 00:33:14.910
Shubo Zhang: document all applications and associated data assets larger organization should consider implementing and configuration management database CD and DVD for this purpose.

155
00:33:15.480 --> 00:33:35.850
Shubo Zhang: establish an application security program and drive adoption, not the capability gap analysis comparing your organization to their peers, to the five key areas and an execution one day management approval and establish an application security awareness campaign for the entire it organization.

156
00:33:42.960 --> 00:33:48.120
Shubo Zhang: Yes, you want to identify prioritize your application from a business perspective.

157
00:33:50.040 --> 00:33:50.610
Shubo Zhang: I promise.

158
00:33:56.040 --> 00:34:05.040
Shubo Zhang: Establish assurance guidelines to properly define the coverage and level of renewal required establishing upon risk rating model will be beneficial.

159
00:34:05.730 --> 00:34:18.660
Shubo Zhang: keep in mind the rating mom should include a consistent set of likely will impact factors reflective of your organization's tolerance for risk every organization is different, so the rating models could differ greatly.

160
00:34:23.700 --> 00:34:34.470
Shubo Zhang: To enable a strong foundation, you need to establish a set of focus policies and standards that providing application security baseline for all development teams to review to.

161
00:34:35.490 --> 00:34:46.440
Shubo Zhang: But you really do that it's important to define a common set of reusable security controls the compliment these policies and standards was by design and development guidance on their views.

162
00:34:47.040 --> 00:34:58.230
Shubo Zhang: Accurate the security controls have been properly defined, you can then establish an application security, training curriculum that was required and targeted to different development roles and topics.

163
00:35:08.280 --> 00:35:22.590
Shubo Zhang: You should define that in grade security implementation verification activities into existence development and operational processes by building security in round out the overall security of the organization and.

164
00:35:23.640 --> 00:35:27.990
Shubo Zhang: There are all sorts of verification activity integrate into your processes.

165
00:35:29.940 --> 00:35:32.400
Shubo Zhang: right on your design review.

166
00:35:33.870 --> 00:35:45.060
Shubo Zhang: Review penetration testing and remediation you should also provide subject matter, experts and support services for development and project teams to be really successful.

167
00:35:51.180 --> 00:35:59.100
Shubo Zhang: it's best to manage your metrics by the government and funding decisions based upon the veterans and analysis data capture.

168
00:36:00.150 --> 00:36:09.990
Shubo Zhang: metrics include adherence to security practices and activities vulnerabilities introduced vulnerabilities mitigated application coverage.

169
00:36:10.410 --> 00:36:18.360
Shubo Zhang: defect density by type and instance counts etc analyze data from the implementation and verification that should be.

170
00:36:18.840 --> 00:36:31.710
Shubo Zhang: Looking for root cause and vulnerability patterns, which will drive strategic and systemic improvements across the enterprise learn from mistakes and offer positive incentives to promote improvements.

171
00:36:38.730 --> 00:36:49.260
Shubo Zhang: Applications belong to the most complex systems humans regularly create and maintain it management for an application should be performed by IT specialists.

172
00:36:49.740 --> 00:37:00.540
Shubo Zhang: who are responsible for the overall it life cycle of the application just establishing the goal of application manager as technical counterpart to the application on.

173
00:37:01.050 --> 00:37:12.540
Shubo Zhang: The application manager is in charge of the whole application lifecycle from the it perspective from the licensing requirements until the process of retiring systems, which is often overlooked.

174
00:37:32.100 --> 00:37:40.620
Shubo Zhang: We should collect them negotiate the business requirements for an application with the business, including the protection requirements with regard to confidentiality.

175
00:37:42.000 --> 00:37:54.480
Shubo Zhang: integrity and availability of all the assets and we expected business logic, then you meet compile the technical requirements include functional and non functional security requirements.

176
00:37:55.200 --> 00:38:03.480
Shubo Zhang: plan and negotiate the budget that covers all aspects of design bill testing and operation, including security actually.

177
00:38:12.810 --> 00:38:28.830
Shubo Zhang: negotiate the requirements with internal or external developers painful new guidelines and security requirements with respect to your security programs like lc lc best practices great films all technical requirements planning.

178
00:38:34.260 --> 00:38:35.760
Shubo Zhang: Security and service levels.

179
00:38:38.970 --> 00:38:39.690
Shubo Zhang: and check with.

180
00:38:41.220 --> 00:38:42.600
Shubo Zhang: Your software conference.

181
00:38:46.500 --> 00:38:51.750
Shubo Zhang: Contract Law, please consult your advice before music the sample in.

182
00:38:58.230 --> 00:39:03.870
Shubo Zhang: negotiate planning and design with the developers and internal shareholders like security specialist.

183
00:39:04.680 --> 00:39:15.240
Shubo Zhang: defined security architecture controls and countermeasures appropriate to the protection and the expected threat model, this should be supported by security specialists.

184
00:39:16.140 --> 00:39:29.550
Shubo Zhang: You should worry about the application assessment name risks or provides additional resources in each sprint ensure security stories are created that include constraints added or non functional requirements.

185
00:39:36.300 --> 00:39:40.680
Shubo Zhang: automate the security of the application interfaces and all requires.

186
00:39:42.630 --> 00:39:43.410
Shubo Zhang: optimization.

187
00:39:44.730 --> 00:39:45.180
Shubo Zhang: Technical.

188
00:39:46.770 --> 00:40:00.480
Shubo Zhang: The IP architecture and coordinate Business Test create use and abuse test cases from technical and business perspective managed security tests according to internal processes, the protection means.

189
00:40:04.410 --> 00:40:13.440
Shubo Zhang: application and operation and migrate from previous have used applications if needed finalize all documentation, including the change.

190
00:40:16.050 --> 00:40:17.790
Shubo Zhang: and security architecture.

191
00:40:24.990 --> 00:40:30.630
Shubo Zhang: operations must include guidelines for the security management of the application like patch management.

192
00:40:31.290 --> 00:40:36.690
Shubo Zhang: raise security awareness of users and manage conflicts that usability versus security.

193
00:40:37.410 --> 00:40:56.730
Shubo Zhang: plan and manage changes like migrating to new versions of the application or other components like os middleware and libraries update all documentation, including in the cmt be and the security architecture controls and countermeasures equally any run books or project documentation.

194
00:41:05.250 --> 00:41:20.910
Shubo Zhang: Any required data should be archived all other data should be securely white securely retiring the application, including the leading unused accounts roles and permissions set your application state to retired in the cm db.

195
00:41:31.110 --> 00:41:47.790
Shubo Zhang: use a wasp as your roadmap to assess your unique situation identify threats and figure out which countermeasures will be the most effective for your particular applications use oh was the most figure out if your number one problem really is your number one.

196
00:41:50.010 --> 00:41:50.310
Shubo Zhang: way.

197
00:41:53.070 --> 00:41:53.460
Shubo Zhang: to your.

198
00:41:55.800 --> 00:42:01.260
Shubo Zhang: Policy and which is essential to properly evaluate your application security focus.

199
00:42:19.200 --> 00:42:33.900
Shubo Zhang: Well that's about it, we know you have invested a lot of time to this course and, in return, we know that understanding oh os and its principles will give you a significant lead and technical edge in developing strong reliable and secure code.

200
00:42:34.680 --> 00:42:42.690
Shubo Zhang: To keep up on the latest related developments visit oh last.org on a regular basis and do consider joining a local or was chapter.

201
00:42:43.410 --> 00:43:02.100
Shubo Zhang: Oh was chapters foster local discussions of application security around the world, local chapters are free and open to anyone and managed by a set of guidelines, known as the last chapter handle Finally, when you are ready, please take the following oh was top 10 secure programming quiz.

202
00:59:11.340 --> 00:59:11.520
hey.

