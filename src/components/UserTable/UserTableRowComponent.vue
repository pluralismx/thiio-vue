<template>
    <tr v-show="details">
        <td>{{ this.name }}</td>
        <td>{{ this.surname }}</td>
        <td>{{ this.phone }}</td>
        <td>{{ this.email }}</td>
        <td class="d-flex justify-space-around align-center">
            <v-btn @click="toggleEdit">Edit</v-btn>
            <v-btn @click="deleteCustomer">Delete</v-btn>
        </td>
    </tr>
    <tr v-show="edit">
        <td>
            <v-text-field
                label="name"
                density="compact"
                v-model="this.name"
            ></v-text-field>
        </td>
        <td>
            <v-text-field
                label="surname"
                density="compact"
                v-model="this.surname"
            ></v-text-field>
        </td>
        <td>
            <v-text-field
                label="phone"
                density="compact"
                v-model="this.phone"
            ></v-text-field>
        </td>
        <td>
            <v-text-field
                label="email"
                density="compact"
                v-model="this.email"
            ></v-text-field>
        </td>
        <td class="d-flex justify-space-around align-center">
            <v-btn color="primary" @click="toggleEdit">cancel</v-btn>
            <v-btn @click="editCustomer">save</v-btn>
        </td>
    </tr>
</template>
<script>
    import axios from '@/lib/axios';
    export default {
        name: 'UserTableRowComponent',
        props: {
            customer: {
                type: Object,
                required: true
            }
        },
        data () {
            return {
                id: null,
                name: null,
                surname: null,
                email: null,
                phone: null,
                edit: false,
                details: true
            }
        },
        computed: {
            customerUpdated(){
                return this.customer;
            }
        },
        watch: {
            customerUpdated: {
                handler(newVal){
                    this.id = newVal.id;
                    this.name = newVal.name;
                    this.surname = newVal.surname;
                    this.phone = newVal.phone;
                    this.email = newVal.email;
                },
                immediate: true,
                deep: true
            }
        },
        methods: {
            toggleEdit(){
                if(this.edit == false){
                    this.edit = true;
                    this.details = false;
                }else{
                    this.edit = false;
                    this.details = true;
                }
            },
            editCustomer(){

                const data = {
                    name: this.name,
                    surname: this.surname,
                    phone: this.phone,
                    email: this.email
                }

                let customer = JSON.stringify(data);
                let formData = new FormData();
                formData.append('json', customer);
                formData.append("_method", "put");

                let url = "api/customers/update/"+this.id;
                let token = localStorage.getItem('token');

                axios.post(url, formData, {headers: {
                    'Authorization': `${token}`
                }})
                .then(res=>{
                    if(res.data.status === 'success'){
                        this.edit = false;
                        this.details = true;
                    }
                })
                .catch(error=>{
                    console.log(error);
                });

            },
            deleteCustomer(){

                let url = "api/customers/delete/"+this.id;
                let token = localStorage.getItem('token');
                axios.delete(url, {headers:{
                    'Authorization':`${token}`
                }})
                .then(res=>{
                    console.log(res.data);
                    if(res.data.status === 'success'){
                        this.$emit('customer-updated');
                    }
                })
                .catch(error=>{
                    console.log(error);
                });
            }
        }
    }
</script>
<style scoped>
    td {
        height: 4rem !important;
    }
</style>