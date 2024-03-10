<template>
    <UserTableComponent :customers="customers_list" @update-customers="handleUpdateCustomers"/>
</template>
<script>
import UserTableComponent from './UserTable/UserTableComponent';
export default {
    name: 'DashboardComponent',
    components: {
        UserTableComponent
    },
    props: {
        customers: {
            type: Object,
            required: true
        }
    },
    data () {
        return {
            customers_list: {}
        }
    },
    computed: {
        updateCustomers(){
            return this.customers;
        }
    },
    watch: {
        updateCustomers: {
            handler(newVal){
                this.customers_list = newVal;
            },
            immediate: true,
            deep: true
        }
    },
    methods: {
        handleUpdateCustomers(){
            this.$emit('update-customers');
        }
    }

}
</script>