<template>
    <v-navigation-drawer>
        <v-list>
            <v-list-item 
                title="Add record"
            ></v-list-item>
            <v-list-item>
                <v-text-field
                    label="name"
                    density="compact"
                    v-model="name"
                ></v-text-field>
            </v-list-item>
            <v-list-item>
                <v-text-field
                    label="surnname"
                    density="compact"
                    v-model="surname"
                ></v-text-field>
            </v-list-item>
            <v-list-item>
                <v-text-field
                    label="email"
                    density="compact"
                    v-model="email"
                ></v-text-field>
            </v-list-item>
            <v-list-item>
                <v-text-field
                    label="phone"
                    density="compact"
                    v-model="phone"
                ></v-text-field>
            </v-list-item>
            <v-list-item>
                <v-btn color="primary" @click="addCustomer">Add</v-btn>
            </v-list-item>
            <v-list-item>
                <p v-show="error" style="color: red;">All fields are required</p>
            </v-list-item>
        </v-list>
        
    </v-navigation-drawer>
</template>
<script>
    import axios from '@/lib/axios';
    export default {
        name: 'DrawerComponent',
        data() {
            return {
                name: null,
                surname: null,
                email: null,
                phone: null,
                error: false
            }
        },
        methods: {
            addCustomer() {

                if(this.name!=null&&this.surname!=null&&this.email!=null&&this.phone!=null){
                    const data = {
                        name: this.name,
                        surname: this.surname,
                        email: this.email,
                        phone: this.phone
                    }

                    let token = localStorage.getItem('token');

                    let customer = JSON.stringify(data);

                    let formData = new FormData();
                    formData.append('json', customer);
                    axios.post('api/customers/save', formData, {headers: {
                        'Authorization': `${token}`
                    }})
                    .then(res=>{
                        if(res.data.status == 'success'){
                            this.$emit('update-customers');
                            this.name=null;
                            this.surname=null;
                            this.email=null;
                            this.phone=null;
                            this.error=false;
                        }
                    })
                    .catch(error=>{
                        console.log(error);
                    });
                }else{
                    this.error=true;
                }
            }
        }
    }
</script>