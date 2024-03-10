<template>

    <v-container>
        <v-card>
            <v-card-title>Log in</v-card-title>
            <v-card-subtitle v-show="!error&!credentials_error">Enter your email and password to log in</v-card-subtitle>
            <v-card-subtitle style="color: red" v-show="error">All fields are required</v-card-subtitle>
            <v-card-subtitle style="color: red" v-show="credentials_error">Invalid credentials</v-card-subtitle>
            <v-form>
                <v-card-text>
                    <v-text-field label="Email" v-model="email"req></v-text-field>
                    <v-text-field 
                        :type="show1 ? 'text' : 'password'" 
                        label="Password" 
                        v-model="password"
                    ></v-text-field>
                </v-card-text>
                <v-card-actions class="d-flex flex-column justify-space-evenly">
                    <v-btn color="primary" @click="logIn()">continue</v-btn>
                    <p @click="signUp">Need an account?</p>
                </v-card-actions>
            </v-form>
        </v-card>
    </v-container>

</template>

<script>

    import axios from '@/lib/axios';

    export default {
        name: 'LoginComponent',
        data() {
            return {
                email: null,
                password: null,
                error: false,
                credentials_error: false
            }
        },
        methods: {
            logIn(){

                if(this.email != null && this.password != null){
                    const credentials = {
                        email: this.email,
                        password: this.password
                    }

                    let credentialsData = JSON.stringify(credentials);
                    let formData = new FormData();
                    formData.append('json', credentialsData);

                    axios.post("api/login", formData)
                        .then(res=>{
                            if(res.data.status == 'error'){
                                this.credentials_error = true;
                            }else{
                                localStorage.setItem('token', res.data);
                                this.$emit("user-logged-in");
                            }
                        })
                        .catch(error=>{
                            console.log(error);
                        });
                }else{
                    this.error = true;
                }
            },
            signUp(){
                this.$emit('sign-up');
            }
        }
    }

</script>

<style scoped>

    p:hover {
        cursor: pointer; 
    }

</style>