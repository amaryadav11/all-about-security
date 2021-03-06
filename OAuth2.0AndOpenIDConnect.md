
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
1.Delegated Authorization: this means how can i let a website access my datawithout giving it my password and this is what OAuth tries to solve and it excels in this field.
2.Resource owner : represents the user who is sitting infront of the keyboard who owns the data the app is looking for
3.client: the app which wants to get the resource owner data
4.Authorization server: who asks user for username and password and show a consent screen to user like accounts.google.com
5.Resource server: the server which holds the user data the client is interested in like google contacts api
6.Authorization  grant: the whole dance from webapp to accounts.google.com to again to webapp is to get this authorisation grant it proves that the user has clicked ok
7. Redirect uri: once the user clicks ok the authorization server needs to know where to redirect the user to in actual webapp this place is called redirect uri
8. Access token: At the end what client actually needs is the access token that allows client to access user data



Everyone of us uses OAuth Authentication in one or other way like when you see login with google or facebook option on a website this is OAuth authentication this is a very common pattern on internet now.So when you click on that button it shows a consent screen and when you click on ok it logs you in and now this new app is connected to your google or facebook account and can access your data from your google or facebook account(note it will not be able to access all of your data it will only
be able to access those that you allow it to see so from next time carefully read the message on consent screen before blindly clicking on ok buton it can access your entire data or delete data if
you provide more permissions to the app) this pattern is called OAuth pattern


when you go to a website and you see a login with google button and you click on that button you have started and OAuth flow(is a set of steps that ultimately results in application being able to access
your data) the users browser  will be redirected to google domain at google domain they will be prompted to login probably it will ask for username and password again this authentication ca be of different types like otp based auth or push authentication or kerberos authentication etc. once the user is authenticated the the user is presented with a prompt hey xyz app want to access a list of things from you account do you want to allow if user clicks yes the browser is redirected to the orginal application where it was started to a special place in application called callback or redirect URI and some magic happens behind the scenes(more about it later) and then that app is allwed to talk to some other api like google contacts api now this app has a special magical thing you see this and give me this user details yes I see it and it looks legit so here is the user details you requested.


1.when the user is redirected to the authorisation server the xyz also passes some info like redirect uri and response type:code(what type of authorization code you want there are few different types of authorization grant most common is authorization code grant at the end xyz app will get a code)

2. the user logs in and click ok
3. user is redirected to xyz app (redirect uri) and is redirected with authorization code(magical thing which we we talking about above) using the authorization code client can't do anything 
now the client one more time goes to the authorization server and say you just send me this authorization code and here is my client id give me an access token this happens on back channel server side this add an extra layer of security because if someone steals the authorization code with xyz client id it is of no use Authorization server verifies the authorisation code and client id it checks that authorisation code is not forged and is still valid now the client has access token it can access user data using this token

with OAuth we can give granular permission to apps this idea in OAuth is called scopes
the authorization server has a list of scopes that it understands like contacts-read, contacts-write etc any type of permission that makes sense in the system so when the client kicks off the flow it passes this scope to Authorization server client can alo requests multiple scopes based on the list of scope he authorization server generates consent screen which is shown to user.

The access token thst comes out of this flow is scoped

Front channel(communication between browser and authorization server) we cannot store api key in js because someone can inspect js code and can misuse it so i will store it on server side so no one can
access it browser cannot be fully trusted bacause info can leak from browser 
back channel(communication between server side code and Authorisation or resource server) we have control over the code running on server and no one can see or change it so more secure and can be fully trusted

why cant we get the access token in first request itself the flow is designed like thi to use the good things of both back and front channel and to make it highly secure

the authorization code comes back to the redirect uri over browser

access token is a sensitive information that also lives on backend server and the resource is fetched using access token from back channel

Steps to setup oAuth
1. register with a Authorization server
after registering app it will give clientid(identifies the xyz app) and client secret(sensitive other part of key that is used with auth code to get get access token)
oAuthdebugger.com

There are a coupe of different OAuthe grant types
1.Authorization code (front channel + back channel)
2. Implicit(front channnel only)(when you dont have a back channel skip the authoeization code exchange step and give me the access token right away from Authorization server dont give me the 
code )(use cases: pure react or angular app that has no backend or mobile app means there is no backend server to render the page or run any sort of logic on server side) response type: token
3. Resource owner password credentials(back channel only) server side code posting credentials to the 
Authorization server to get the access token right away no browser involved used when working with older apps
4. client credentials (back channel only) used when doing machine to machine or service to service communication

Identity use cases(pre 2014)
1.Simple login (Oauth 2.0)
2. SSO(Oauth 2.0)
3. Mobile app login(Oauth 2.0)
3. Delegated Authorization(Oauth 2.0)

Login to an app using social credentials
OAuth was built for Authorization not for Authentication but people started using it for authentication use cases Oauth was never designed for authentication use cases(its a hack)

using oauth for authentication is bad because in oauth there is no standard way of getting user
info

when an user logs into an app an app may want to know who just logged in Oauth was designed for permissions scopes it cares about is you access token scoped for particular thing it does not care who you are when google or facebook made their login with goolge button they had their own way to get the user info that oauth was missing

OpenID Connect(protocol) is an extension to Oauth 2.0 that provides standard way for authentication

OpenID Connects adds
1. ID token - it represents the id or info about the user provided by authorisation serever jwt token
jsonwebtoken.io
2. user info endpoint for getting more user information that is not available in id token which all
openid connect enabled authorization servers implement which means if i am taking to an authorization
server which is openid enabled or understands openid connect i can also ask for an id token(contains some user info) 

in request we add an extra scope: openid so this means its an openid request insteat of plain oath request

now when we exchange code with token now we also get an id token it can be decoded by the application to understand who the user is who just logged in we can also use the access toke to get more info about user by calling userinfo endpoint openid connect debugger
3. standard set of scopes
4. standardized implementation

JWT structure 
header section:
payload section: contains claim it is decoded by app to understand who just logged in when token expires when they logged in etc
signature portion: verify id token has not been modified or compromised or changed(forged) in any way during flight it verifies using public key of authorization server

use Oauth for granting access to your api getting access to user data in other system(authorization)


use openid for for user login , making your accounts available in other systems

whic grant type(flow) do i use?
web app with backend recommended authorization code flow
native mobile app: authorization code flow with PKCE
javascript app(SPA) with api backend: implicit flow
microservices and apis: client credentials flow


oauth.com - ebbok

developer.okta.com to host your own authorisation server
