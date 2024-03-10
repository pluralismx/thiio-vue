<template>
    <v-app>
        <NavigationBarComponent
            :available="loginButton"
            @user-logged-out="handleLogOut"
        />
        <DrawerComponent v-if="token !=null"
            @update-customers="loadCustomers"
        />
        <v-main>
            <!-- Login Screen -->
            <LoginComponent v-if="token === null && !toggleSignUp" @user-logged-in="handleUserLoggedIn" @sign-up="handleSignUp" />
            
            <!-- SignUp from -->
            <SignUpComponent v-show="toggleSignUp" 
                @sign-up-cancelled="handleSignUpCancelled"
                @user-created="handleUserCreated"
            />

            <!-- Table -->
            <DashboardComponent v-if="token" :customers="customers"
                @update-customers="loadCustomers"
            />
            <SignUpAlertComponent v-show="toggleSignUpMessage" />
        </v-main>
    </v-app>
</template>

<script>
import axios from '@/lib/axios';
import NavigationBarComponent from './components/NavigationBarComponent';
import LoginComponent from './components/LoginComponent';
import DrawerComponent from './components/DrawerComponent';
import DashboardComponent from './components/DashboardComponent';
import SignUpComponent from './components/SignUpComponent';
import SignUpAlertComponent from './components/SignUpAlertComponent';

export default {
    name: 'App',
    components: {
        NavigationBarComponent,
        DrawerComponent,
        LoginComponent,
        DashboardComponent,
        SignUpComponent,
        SignUpAlertComponent
    },
    data() {
        return {
            token: null,
            toggleSignUp: false,
            toggleSignUpMessage: false,
            customers: [],
            loginButton: false,
        }
    },
    created(){
        this.handleUserLoggedIn();
    },
    methods: {
        handleUserLoggedIn() {
            let token = localStorage.getItem('token');
            if (token && token != undefined) {
                this.token = token;
                this.loginButton = true;
                this.loadCustomers();
            } else {
                this.token = null;
            }
        },
        handleSignUp(){
            if(this.toggleSignUp === false){
                this.toggleSignUp = true;
            }else{
                this.toggleSignUp = false;
            }
        },
        handleSignUpCancelled(){
            this.toggleSignUp = false;
        },
        handleUserCreated(){
            this.toggleSignUp = false;
            this.showSignUpMessage();
        },
        showSignUpMessage(){
            this.toggleSignUpMessage = true;
            setTimeout(()=>{
                this.toggleSignUpMessage = false;
            }, 4000);
        },
        handleLogOut(){
            this.token=null;
            this.loginButton = false;
        },
        loadCustomers(){

            axios.get("api/customers/records", {headers: {
                'Authorization': `${this.token}`
            }})
            .then(res => {
                this.customers = res.data.customers;
            })
            .catch(error => {
                console.log(error);
            });
        }
    }
}
</script>
