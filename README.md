#sails-PassportLocalMongoExample
This is a working example of how to get authentication working on sailsjs 0.9.

This uses the following npm libraries:

- passport
- pasport-local
- mongodb
- bcrypt
- sails-mongo

##Setup & Install
You need to have sailsjs 0.9 installed locally and linked via NPM..


###Installing sailsjs 0.9
cd to a directory where you want to store the sails clone.. I use `~/Web/Frameworks`..

`git clone http://github.com/balderdashy/sails.git -b development sails; cd $_; npm link;`

###Clone the repository, and cd into its folder:
I store all my web projects in `~/Web/Sites/`..

`git clone https://github.com/RelativeMedia/sails-PassportLocalMongoExample.git sails-auth-example; cd $_;`

###Run NPM Link, to link the sails module defined to your local version..

`npm link sails`

###Run NPM Install:

`npm install`

###Install mongodb, and start it with `mongod`
I wont outline this step as it can vary from system to system. For me (on osx 10.8) it was just using brew to install

**Note:** if you want to use disk based db, change the `default: 'mongo'` to `default: 'disk'` in the  `/config/adapters.js` file.

run `sails lift` in the app's directory


##Getting it Working
With the server running.

Create a User by POSTing to `/user` with the following:

- username
- password

You should get a JSON response back of the user you just created.

then try to access via GET `/user`. You should get a JSON response of "Not Authorized"

Now POST to `/login` with the following fields:

- username
- password

You should get a json response of "Login Successful"

then again try to access via GET '/user'. You should get a JSON response of the users in the database (in this example, only one.)

You will notice that the password is not returned.

### Logout
Logout is now working, just simply access `/logout`

##Todos
- Enable CSRF

##Explanation of Files, and what they do

- `/config/passport.js` injects the custom middleware for passport into express
- `/config/policies.js` this is where you define what controllers and actions are locked down by passport
- '/config/routes.js` routes were setup for login and logout
- `/api/controllers/AuthController.js` handles login/logout
- `/api/controllers/UserController.js` handles user CRUD
- `/api/models/Auth.js` has a beforeCreate method which runs the plaintext password through bcrypt, also strips out the password field from json responses
- `/api/services/passport.js` the meat and potatoes of getting passport to work, this is where we find the user, and perform validation for login/logout
- '/api/policies/passport.js` Session validation file, makes sure users are actually logged in when needed
