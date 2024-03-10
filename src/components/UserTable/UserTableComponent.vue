<template>
    <v-container>
        <v-table>
            <thead>
                <tr>
                    <th style="width: 20%;">Name</th>
                    <th style="width: 20%;">Surname</th>
                    <th style="width: 20%;">Phone</th>
                    <th style="width: 20%;">Email</th>
                    <th style="width: 20%;">Actions</th>
                </tr>
            </thead>
            <tbody>
                <UserTableRowComponent v-for="customer in customers_list" :key="customer.id" :customer="customer"
                    @customer-updated="handleCustomersUpdated" 
                />
            </tbody>
        </v-table>
    </v-container>
</template>
<script>
import UserTableRowComponent from './UserTableRowComponent';

export default {
    name: 'UserTableComponent',
    components: {
        UserTableRowComponent
    },
    props: {
        customers: {
            type: Object,
            required: true
        }
    },
    data() {
        return {
            customers_list: {}
        }
    },
    computed: {
        customersUpdated() {
            return this.customers;
        }
    },
    watch: {
        customersUpdated: {
            handler(newVal) {
                this.customers_list = newVal;
            },
            immediate: true,
            deep: true
        }
    },
    methods: {
        handleCustomersUpdated() {
            this.$emit('update-customers');
        }
    }
}
</script>