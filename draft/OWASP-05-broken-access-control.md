
## What do you need to know about broken access control

Exploitation of access control is a core skill of attackers.

SAST and DAST tools can detect the absence of access control, 
but cannot verify if it is functional when it is present.


Access controls is detectable using manual means,  or possibly through automation for the absence of access controls in certain frameworks.


Access control weaknesses are common due to the lack of automated detection and lack of effective functional testing by 
application developers.


Access control detection is not typically amenable to automated, static, or dynamic testing.


Manual testing is the best way to detect missing or ineffective access control, including http method (GET vs PUT) 
controller, direct object references, etc.


The technical impact is attackers acting as users or administrators for users accessing privileged functions for creating 
accessing updated or deleting every record.

The business impact depends on the protection needs of the application and data.


## Is your application vulnerable
access control and horses policy, such that users cannot act outside of the intended permissions they all yours typically 
lead to an authorized relations corrosion multiplication or destruction of data or performing the business function outside of the limits of the user.


Common access control vulnerablilites include bypassing access control checks by modifying the URL, internal application state, 
or the html page,  or simply using a custom API attack tool.


Allow the primary key to change to another user's record committing viewing or editing someone else's account.


Elevation of privilege. Acting as a user, without being logged in, or acting as an admin when logged in as a user.


Metadata manipulation, such as replaying or tampering with a json web token JWT,  access control token, or a cookie,
or a hidden field manipulated to elevate privileges, or abusing JWT invalidation.

CORS missconfiguration allows unauthorized API access.


Force browsing to authenticated pages as an authenticated user or to privileged pages as a standard user. 
Accessing API with missing access controls for POST, PUT, and DELETE.

## Examples
### scenario one the application uses unverified data in a sequel call that is accessing account information.


And attacker simply modifies the account parameter in the browser to send whatever account number, they want, if not properly 
verified the attacker can access any account.


### scenario two an attacker simply forces browsers to target URLs admin rights are required for access to the admin page 
if an authenticated user can access either page it's a flaw if a non admin can access the admin page, this is a block.


access control is only effective they've been forced to trusted server side code or server list API or the attacker cannot modify the access control 
check or metadata there are some techniques, you can follow to prevent broken access control.


With the exception of public resources denied by default.


Implement access control mechanisms once and we use them throughout the application, including minimizing portion usage.


model access, controls should enforce record ownership, rather than accepting that the user can create read update or delete any record.

meet application business limit requirements should be enforced by domain models.


disable web server directory listing and ensure file metadata like dot govt and backup files are not present within one groups.

log access control failures alert admins who are the appropriate like repeated failures.


rate limit API and controller access to minimize the harm from automated attack tooling.


J wt tokens should be invalidated on the server after log APP.

developers and QA SAP should include functional access control unit and integration tests.
