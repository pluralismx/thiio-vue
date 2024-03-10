<template>
    <v-app-bar>
        <v-app-bar-title>Welcome</v-app-bar-title>
        <v-btn v-show="available" class="primary" @click="logOut()">Log out</v-btn>
    </v-app-bar>
</template>
<script>
export default {
    name: 'NavigationBarComponent',
    props: {
        available: {
            type: String,
            required: true
        }
    },
    data(){
        return {
            available_updated: false
        }
    },
    computed: {
        updateAvailable(){
            return this.available;
        }
    },
    watch: {
        updateAvailable: {
            handler(newVal){
                this.available_updated = newVal;
            },
            immediate: true
        }
    },
    methods: {
        logOut(){
            localStorage.removeItem('token');
            this.$emit('user-logged-out');
        }
    }
}
</script>