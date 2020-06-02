
Learn Oauth2.0 and OpenID Connect in simple terms without any fancy technical terms and programmers jargons.


First I'll start with basics of Web Security 
Authentication, Authorization & Simple forms of authentication used in Web Applications.


Authentication: The process of establishing user Identity. In simple terms in this process web app
ask user to prove its identity to server means The server way of saying who are you? can you tell me your username and password so that i can identify you.

Authorization: The process of checking if a user is allowed to access a resource he is trying to access
meaning the server way of saying I know you i see that in my database there is user with that name and password but wait let me check if you can access the resource you are accessing, sorry you are
not an Admin i cant allow you to access the requested resource please login with admin credentials or 
talk to administrator if you need to access this resource.


Simple Form based authentication: Web app presents the user with a username & pasword form the user
enters the values then it is sent to server for verification the server side code will hit the database
check if the user provided information matches with the username password(hashed using good hashing algo like bcrypt or PBKDF2 etc) stored in database if every thing is correct the user is authenticated. now it will get the user info and then it wil get the user authorization info if the user is authorized to
access the resource it will drop a cookie that contains session id in browser and from now ownwards user can use the app. on subsquent request browser will send the cookie to the server in this way server remembers the user and does not asks for password again and this is how sever  can keep track of logged in user.

This is how authentication started on web and is still used today in many web applications on
internet, but there is some downsides to it as this approach  in not scalable and is not so useful as
today most of the internet traffic is generated from mobile based applications and since the developer or the one who owns the web app is responsible for maintainance(make sure it is up and running) and security(upgrading to latest hashing algo if any vulnerabilty is found in the hashing algo used to hash the password or prevent against unauthorised access to this sensitive user data) of the authentication system plus it does not fulfill all  the current modern web app requirements
like delegated authorization(don't worry more on delegated authorization later),  SSO, mobile app authentication etc.

Identity use cases (pre 2010)
Simple login(form & cookies)
Sinngle Sign-on accross sites(SSO) (authenticate once with a centralized authentication system and use
multiple web applications without entering  user credentials on each website)- SAML
mobile app login - no standard solution available
Delegated authorization- no standard or good solution available

and OAuth and OpenID Connect tries to solve all the above problems in a efficient secure and  standardised way.
OAuth2.0 Terminologies and Jargons.


