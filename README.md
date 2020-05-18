# Code Tutorial

## Description

A reverse engineering of the starter code found in the 'Develop' folder of this repo, explaining every file's responsibility, purpose, and connection to other files.

### User Story

```
AS A developer
I WANT a walk-through of the codebase
SO THAT I can use it as a starting point for a new project

WHEN I follow the walkthrough
THEN I understand the codebase
```
## Table of Contents

* [Develop](#Develop)
* [Config](#Config)
  * [Middleware](#Middleware)
* [Models](#Models)
* [Public](#Public)
  * [JS](#JS)
  * [Stylesheets](#Stylesheets)
* [Routes](#Routes)
* [Adding Changes](#Adding-Changes)

## Develop

The 'Develop' folder is the main directory of the project. It contains all necessary folders and files to run the application. For this particular example, those would be the 'config', 'models', 'public', and 'routes' folders along with the files and any subfolders they contain.

There are only two stand-alone files in this 'Develop' folder: package.json and server.js 

___________________________________________________________________________________
**package.json** 

This file contains information and properties of the application in JSON format. This information is first generated when running `npm init` at the start of a project. A very important part of this file is the information under "dependencies", which is a list of all npm packages that support and add extra functionality to Node applications.

```
  "dependencies": {
    "bcryptjs": "2.4.3",
    "express": "^4.17.0",
    "express-session": "^1.16.1",
    "mysql2": "^1.6.5",
    "passport": "^0.4.0",
    "passport-local": "^1.0.0",
    "sequelize": "^5.8.6"
  }
}
```

On running `npm install` in the terminal, all of these dependencies will be installed.
___________________________________________________________________________________
**server.js**

This JavaScript file is the eventual endpoint for all folders and files. On running `npm start` or `node server.js` in the terminal, the code in this file is run, which does the following:

1. Creates a server using Express.    
  By requiring the Express module in line 2, we can create the 'app' variable and use Express methods to create our server. The 'express-session' module imported on line 3 is used in conjunction with Express methods to keep track of the user's login status. 

2. Sets up the port for the local host to access the server.   
  Line 8 defines the port number, which is used in line 27 to start listening on said port once the database is synced (line 26). 

3. Imports passport.js for request authentication (line 5).  
  Express then uses this customn-configured passport in lines 18 and 19, to initialize authentication and allow the user persistent login sessions.

4. Imports database table models (line 6).  
  The 'db' variable is then called in line 26 to create database tables in sync with the server being started.

5. Imports html and api routes (lines 22 & 23)  
  Requiring our route files returns a function, in which the 'app' variable is then passed in as a parameter (in the second paranthesis).
___________________________________________________________________________________

## Config

___________________________________________________________________________________
**config.json**

Contains database config variables for different environments (development, testing, and production).
___________________________________________________________________________________
**passport.js**

1. Imports the 'passport' and 'passport-local' npm packages (line 1 & 2) which provide authentication using a username and password in Node.js applications using Express.
  
2. Imports the database models (line 4).
___________________________________________________________________________________

### Middleware

___________________________________________________________________________________
**isAuthenticated.js**

This is a custom middleware file that restricts routes to users that are not logged in. It exports a function that checks if a user tyring to access a route is loggin in or not. If the condition is met, the request continues on to the next route. If the condition is not met, the user is redirected to the '/' HTML route, which direct send the signup.html page.
___________________________________________________________________________________

## Models

___________________________________________________________________________________
**index.js**
_______________________________________________________________________________________
**user.js**
_______________________________________________________________________________________

## Public

The 'public' folder had subfolders for js and css files, the html files are standalone. In the head of each html file, Bootstrap and the custom style.css are linked on lines 8 and 9.

___________________________________________________________________________________
**login.html**

This is the view for the login screen.

The body has a login form with inputs for email and password. When clicking the 'login' button, the form input values are submitted and handled by the login.js file that is linked on line 41. JQuery is linked on line 40 to support the login.js file.

If user needs to sign up for an account first, that can hit the link under the form to navigate to the sign up html page.
___________________________________________________________________________________
**members.html**

This is the view after a successful user login. 

This view has a 'logout' link in the navbar. The welcome message on line 25 has an empty span that will have the dynmically rendered user name after login. The javascript for this file is link on line 31, member.js
___________________________________________________________________________________
**signup.html**

This is the signup view. 

The body had a signup form with inputs for email and password. There is also an area for dynamic error messages to be displayed if user signup fields do not meet validation or already exist. When clicking the 'sign up' button, the input values are submittted and handled by the logic in signup.js (linked on line 45).

Users can navigate to the login screen HTML page by clicking the link below the signup form.
___________________________________________________________________________________

### JS 

These JavaScript files handle frontend logic. They rely on the jQuery library which is linked in their cooresponding HTML files.

___________________________________________________________________________________
**login.js**

This file sets up the event listener for the 'login' form in login.html  
It calls the loginUser function, passing in the form input values for email and password Those values are then used to run a POST request to the 'api/login' route, which authenticates the use login and redirects the user to the members page. Any error that occur during login are logged in the console.
___________________________________________________________________________________
**members.js**

This file does a GET request to the 'api/user_data' route for data on the user that is logged in for that session, and uses that user's email to populate the text for the empty span tag in members.html.
___________________________________________________________________________________
**signup.js**

This file sets up the event listener for the 'sign up' form in signup.html  
It calls the signUpUser function, passing in the sign up form values for email and password. Those values are then used to run a POST request to add the user to the database via the 'api/signup' route and then redirects the uer to the members page. An errors that occure during sign up are rendered in a Boostrap alert as HTML.
___________________________________________________________________________________

### Stylesheets

___________________________________________________________________________________
**style.css**

This file has minimal custom css (only customizes margins). Most CSS styling for the HTML files is done with Bootstrap classes.___________________________________________________________________________________

## Routes

___________________________________________________________________________________
**api-routes.js**

This JavaScript file sets up API routes and handles requests made to each of those routes. 

1. '/api/login'    
  This route requires the passport.js file in the 'config' folder

2. '/api/signup'  

3. '/logout'  

4. '/api/user_data  
___________________________________________________________________________________
**html-routes.js**

This JavaScript file sets up HTML routes. All HTML routes use the built-in 'path' Node module to enable relative route paths (line 2).

1. '/'   
  When first opening the application in the browser, the '/' route will direct unlogged users to the signup.html page. Once a first time user creates an account and is logged in, the '/' route will redirect them to the '/members' route page. Their login session is persistent, which means that if they leave the app and revist, instead of being directed to signup again, they will be directed to members page. This route is also requested when the user clicks on the 'sign up' link at the bottom of the login.html page. 

2. '/login'  
  This route is requested when a user clicks on the 'log in' link at the bottom of the signup.html page. It sends the login.html file, unless that user is already logged in, then they are taken to '/members'.

3. '/members'  
  This route requires the isAuthenticated.js file (line 5). Requests to the '/members' route only go through if the user meets the login condition in the isAuthenticated function. If so, members.html is rendered in the browser.
___________________________________________________________________________________

## Adding Changes

*(At the end of the tutorial, add instructions for how you could now add changes to this project.)*
