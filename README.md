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

This folder contains a config.json and passport.js file, along with a 'middleware' subfolder.

___________________________________________________________________________________
**config.json**

Contains database config variables for different environments (development, testing, and production).
___________________________________________________________________________________
**passport.js**

This file sets up login authentication, and is exported for use in other files (line 49). It:

1. Imports the 'passport' and 'passport-local' npm packages (line 1 & 2).    
  Passport provides authentication using a username and password in Node.js applications. In this application, the username is set up to be an email login instead (line 10).
  
2. Imports the database models (line 4).  
  We access the 'user' data model and call the findOne method in order to find the user with the same email from login. (lines 14 to 17).

3. Validation occurs to check is the email exists, and whether its accompanying password matches what is in the database or not (lines 20 to 30). Validation is done, and returns messages regarding errors or, if none, returns the matching user.
___________________________________________________________________________________

### Middleware

Middleware is software that acts as a bridge between an operating system or database and applications. This subfolder contains a custom middleware file, isAuthenticated.js.
___________________________________________________________________________________
**isAuthenticated.js**

This file restricts routes to users that are not logged in. It exports a function that checks if a user tyring to access a route is loggin in or not. If the condition is met, the request continues on to the next route. If the condition is not met, the user is redirected to the '/' HTML route, which direct send the signup.html page.
___________________________________________________________________________________

## Models

This folder contains two files: index.js and user.js  
These files are used by the application to hold and manipulate data using Sequelize.
___________________________________________________________________________________
**index.js**

This files imports built-in Node modules 'fs' and 'path', as well as the installed npm module 'sequelize'. It exports the 'db' object variable.

Lines 11 to 15 set up the 'sequelize' variable according to the variables in config.json for the 'development' environment by default, and if envirorment is switched, it will use those configurations (like production or testing).

Then, the 'fs' module is used to read file names and paths of all data models (currently, there is just one: user.js) and will import 'sequelize' to all.
_______________________________________________________________________________________
**user.js**

This file requirers the 'bcrypt' npm module (line 2), which provides password hashing. It exports a function that defines the 'User' model table, with 'email' and 'password' columns (and the desired validation/properties for each).

It also adds methods to the 'User' model which compare hashed and unhashed passwords and pre-hash passwords.
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

A subfolder for frontend CSS files.

___________________________________________________________________________________
**style.css**

This file has minimal custom css (only customizes margins). Most CSS styling for these HTML files is done with Bootstrap classes.
___________________________________________________________________________________

## Routes

The 'routes' folder contains all routes that can be requested by the user in the application, both API and HTML.

___________________________________________________________________________________
**api-routes.js**

This JavaScript file sets up API routes and handles requests made to each of those routes. 

1. '/api/login'    
  This route requires the passport.js file to authenticate login and return the matching user. If login is valid, the frontend API call will then redirect the user to the members page.

2. '/api/signup'   
  This route requires 'models' to define a 'db' variable. The sequelize 'create' method is used with the User model to add the new user to the database table with column values from the request body (sign up form inputs). That new user is then directed to '/api/login' and autmatically logged in.

3. '/logout'    
  Passport makes the logout function (line 31) available for the request. It can be called from this route to terminate a login session an removes the user property. The user is then redirected to the '/' route.

4. '/api/user_data  
  If there is a user property (a user is login in), this route will retrieve and send back the users email and id. If there is no user logged in, it returns an empty objects. This data is used to display user's email on the member's page.
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

Now that each file and folder and their interconnectivity is understood, changes can be added with the full scope of their effect in mind. Every changed should be checked to make sure it is reflected in files it is dependent on or that are dependent of it. This will ensure new functionality doesn't break and is fully intergrated into the application.

**Example: Changes to a model**  
To add more data columns to our User table model (like username, first name, last name):
1. In MySQL Workbench, drop any pre-existing 'user' table in the 'passport_demo' database.
1. Add new data properties to 'User' varable in user.js, along with desired valiation.
2. Add input fields for new data properties in signup.html (login will remain just email and password, so no changes needed for login.html or passport.js).
3. In signup.js, grab the new field inputs form the DOM using jQuery as shown in lines 3 to 5.
4. Then, add those input values to the userData variable (line 10) and to the 'if' condition in line 15.   
  Example: `if (!userData.username || !userData.email || !userData.password)`
5. Make sure the signUpUser function (line 26) allows parameters for new data to be passed in.   
  Example (with username being added):
    ```
    function signUpUser(username, email, password) {
        $.post("/api/signup", {
          username: username,
          email: email,
          password: password
        })
    ```
7. Now, we need to make sure API routes match the new changes. In api-routes.js, lines 17 to 20, add new properties to the object being passed into the 'create' method so that those new form input values are used to create the new user.  
  Example:
    ```
    db.User.create({
        username: req.body.username,
        email: req.body.email,
        password: req.body.password
      })
    ```
8. To change the members page to display username instead of email, include the username property in the response object (line 44) of api-routes.js '/api/user-data'. 
9. Then, in members.js, change the text for the empty span to 'data.username'.
10. And that's it! When the server is starated up again, a new user table will be made in teh database including the new changes.
