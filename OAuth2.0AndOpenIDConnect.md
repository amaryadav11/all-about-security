
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

