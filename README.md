<strong>Installation</strong>

1. In your htdocs directory please create a new folder named thiio-fullstack, if you already create it clone this repository there.
2. In your preffered terminal go to the projects directory and run ```npm install``` to update all dependencies
3. Run ```npm run serve``` to compile the application.
4. The default web server's app port is 8080 under your localhost.
5. To start using it, make sure thiio-api backend app is already installed.

<strong>Instructions</strong>

Since the user's table is empty you must sign up to create an account to start using the app, please follow the next steps:

1. Click on "need an account?"
2. Enter your personal info and desired email account
3. Click continue
4. Log in
5. Contacts table is empty, start adding contacts in the form to the left.
6. To delete an item click on the delete button in the contacts table row
7. To update a contact click on edit
8. To log out, click on 'Log out' in the upper right-hand corner.

<strong>Features</strong>

1. Vue.js 3.0
2. Vuetify 3.0
3. Axios 1.6.7

To update the views we used the Vue's life hooks watch and computed to update the data of the desired component's props.
We use the ```data(){return{}}``` option to update components instantly, as well with the help of properties such as immediate in the watch life hook when needed.

<strong>Axios and HTTP requests</strong>

To define the base URL, we created a directory named 'lib' where the axios instance is defined. For the authorization token, we simply access the ```localStorage()``` object to save the token and use it when needed in the axios HTTP requests. If you check all of the axios methods, you will notice that we didn't have to define the API's endpoint HTTP method with the FormData object, except for PUT requests. To accomplish PUT calls to the API method, a spoofing technique was implemented as follows:

1. Call to the POST method in your axios instance like this instead: ```axios.post```
2. In the ```FormData()``` object append the methods name like this ```formData.append("_method", "put");``` assuming  formData contains the ```FormData()``` object
3. Receive the server's response.

<strong>Final notes</strong>
<br/>
<br/>
Vuetify library offers a wide variety of components, which is advantageous as it was specifically developed for Vue.js SPAs. This is particularly beneficial for complex components like data tables. In this example, although we didn't utilize a data table component, we did employ a regular v-table component for buttons and all other elements within the app.
<br/>
<br/>
Thanks for reading this file.
<br/>
<br/>
Sincerly, Gerardo
