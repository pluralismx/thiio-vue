<template> 
    <v-container>
        <v-card>
            <v-card-title>Register</v-card-title>
            <v-card-subtitle v-show="!error&!password_error&!email_error">Create a new user to access</v-card-subtitle>
            <v-card-subtitle style="color: red" v-show="password_error">Passwords must match</v-card-subtitle>
            <v-card-subtitle style="color: red" v-show="error">All fields are requried</v-card-subtitle>
            <v-card-subtitle style="color: red" v-show="email_error">Email already registered</v-card-subtitle>
            <v-card-text>
                <v-text-field label="Name" v-model="name" required></v-text-field>
                <v-text-field label="Surname" v-model="surname"></v-text-field>
                <v-text-field label="Email" v-model="email"></v-text-field>
                <v-text-field :type="show1 ? 'text' : 'password'"  label="Password" v-model="password"></v-text-field>
                <v-text-field :type="show1 ? 'text' : 'password'"  label="confirm password" v-model="verified_password"></v-text-field>
            </v-card-text>
            <v-card-actions class="d-flex justify-space-evenly">
                <v-btn @click="signUp()">continue</v-btn>
                <v-btn @click="cancelSignUp">Cancel</v-btn>
            </v-card-actions>
        </v-card>
    </v-container>

</template>

<script>

    import axios from '@/lib/axios';
    
    export default {
        name: 'SignUpComponent',
        data() {
            return {
                name: null,
                surname: null,
                email: null,
                password: null,
                verified_password: null,
                password_error: false,
                email_error: false,
                error: false
            }
        },
        methods: {
            signUp(){

                if(
                    this.name!=null&&
                    this.surname!=null&&
                    this.email!=null&&
                    this.password!=null&&
                    this.verified_password!=null){
                    
                    var password;

                    if(this.password === this.verified_password){
                        password = this.verified_password;
                        
                        const user = {
                            name: this.name,
                            surname: this.surname,
                            email: this.email,
                            password: password
                        }

                        let userData = JSON.stringify(user);
                        let formData = new FormData();
                        formData.append('json', userData);

                        axios.post("api/register", formData)
                            .then(res=>{
                                if(res.data.status == 'success'){
                                    this.$emit('user-created');
                                    this.password_error = false;
                                    this.error = false;
                                    this.email_error = false;

                                    this.name = null;
                                    this.surname = null;
                                    this.email = null;
                                    this.password = null;
                                    this.verified_password = null;
                                }else if(res.data.status == 'error'){
                                    this.email_error = true;
                                }
                            })
                            .catch(error=>{
                                console.log(error);
                            });
                    }else{
                        this.password_error = true;
                        this.error = false;
                    }
                }else{
                    this.password_error = false;
                    this.error = true;
                }
            },
            cancelSignUp(){
                this.$emit('sign-up-cancelled');
                this.password_error = false;
                this.error = false;
                this.name = null;
                this.surname = null;
                this.email = null;
                this.password = null;
                this.verified_password = null;
            }
        }
    }
</script>