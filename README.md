<strong>Installation</strong>

1. In your htdocs directory please create a new folder named thiio-fullstack, if you already create it clone this repository there.
2. In your preffered terminal go to the projects directory and run ```npm install``` to update all dependencies
3. Run ```npm run serve``` to compile the application.
4. The default web server's app port is 8080 under your localhost.
5. To start using it, make sure thiio-api backend app is already installed.

<strong>Instructions</strong>

Since the user's table is empty you must sign up to create an account to start using the app, please follow the next steps:

1. Click on "need an account?"
2. Enter your personal info and desired account email
3. Click continue
4. Log in
5. Contacts table is empty, start adding contacts in the form to the left.
6. To delete an item click on the delete button in the contacts table row
7. To update a contact click on edit
8. To log out click on log out on the upper right hand corner.

<strong>Features</strong>

1. Vue.js 3.0
2. Vuetify 3.0
3. Axios 1.6.7

<strong>Architecture</strong>

To update the views we used the vue life hooks ```watch``` and ```computed``` so we can update the data of the desired component's ```props```.
We use the ```data(){return{}}``` option to update components instantly, as well with the help of properties such as ```immediate``` in the ```watch``` life hook when needed.

To define the base URL we created a directory named "lib" where the axios instance is defined. For the authorization token we simply access to the ```localStorage``` object to
save the token and use it when needed in the axios http requests.
