# XML External Entities (XXE)
Many older or poorly configured XML processors evaluate external entity reference within XML documents.
External entities can be used to disclose internal files using the file URI handler, internal file shares
, internal port scanning, remote code execution, and denial of service attacks.


83
00:22:57.180 --> 00:23:01.140
What should you know about xml external entities or SMC.

84
00:23:01.860 --> 00:23:14.190
attackers can exploit vulnerable xml processors if they can upload xml or include hostile content in an xml document exploiting vulnerable code dependencies or integrations.

85
00:23:14.910 --> 00:23:25.440
by default many older xml processors a while specifications of an external entity, a you are I, that is the reference and evaluated during xml processing.

86
00:23:26.160 --> 00:23:44.850
that's tools can discover this issue by inspecting dependencies and configuration dash tools require additional manual steps to the technical support position manual testers need to be trained in how to test for X sexy, as it is not commonly tested as of 2017.

87
00:23:45.900 --> 00:24:03.720
These flaws, can be used to extract data executed remote request from the server scan internal systems and or perform a denial of service attack, as well as executed other attacks the business impact depends on the protections of all affected applications and data.

88
00:24:09.510 --> 00:24:31.170
applications, and in particular xml based web services for downstream integrations might be vulnerable to attack is the application, except xml directly for xml uploads, especially from untrusted sources for inserts untrusted data into xml documents which is then parsed by an xml processor.

89
00:24:32.340 --> 00:24:41.820
Any of the xml processors in the application or soap based web services has DVDs or document type definitions enable.

90
00:24:42.570 --> 00:24:54.450
As the exact mechanism for disabled, the TV processing varies by processor, it is good practice to consult a reference, such as the old boss cheat sheet XX see prevention.

91
00:24:55.380 --> 00:25:08.850
If you're African application was assembled for identity processing within federated security for single sign on sso purposes saml uses xml for identity assertions and maybe vulnerable.

92
00:25:09.900 --> 00:25:20.550
If the application uses so prior to version 1.2 it is likely susceptible to XX he attacks the xml entity so being passed through so framework.

93
00:25:21.570 --> 00:25:31.200
be vulnerable to accept the attacks likely means that the application is vulnerable to denial of service attacks, including 2 billion laughs attack.

94
00:25:39.300 --> 00:25:55.710
Numerous public execs the issues have been discovered instantly attacking and then devices and sexy occurs in a lot of unexpected places, including deeply nested dependencies easiest way is to upload malicious xml file accepted.

95
00:25:59.250 --> 00:26:18.990
scenario one the attacker attempts to extract data from the server scenario to an attack improves the servers private network by changing the below entity on scenario three an attacker attempts a denial of service attack by including a potentially endless file.

96
00:26:24.690 --> 00:26:28.170
developer training is essential to identify and mitigate.

97
00:26:29.580 --> 00:26:41.490
Besides that prevented sexy requires that you whenever possible use less complex data formats, such as Jason and avoid serialization of sensitive data.

98
00:26:42.450 --> 00:26:56.160
path to upgrade all xml processors and libraries in use by the application or on the underlying operating system use dependency checkers update so just so 1.2 or higher.

99
00:26:57.210 --> 00:27:20.400
disable xml external entity NDTV processing in all xml horses in the application as per the old boss chichi accepts the prevention implement positive server side input validation filtering or standardization, to prevent possible data within xml documents headers or nodes.

100
00:27:21.930 --> 00:27:31.710
verify that xml or xml file upload functionality validates incoming xml using excess do validation or similar.

101
00:27:33.360 --> 00:27:44.730
SAS tools can help detect Excellency and source code, although manual code review is the best alternative in large complex applications with many integrations.

102
00:27:45.180 --> 00:28:00.030
If these controls are not possible considering using virtual patching API security gateways for web application firewalls wax to detect monitor and block exits he attacks.

