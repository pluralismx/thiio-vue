<template>
    <v-container>
        <v-row>
            <v-alert
                color="success"
                icon="$success"
                title="User created"
                text="User created successfully please log into your account to start testing the app"
            ></v-alert>
        </v-row>
    </v-container>
</template>
<script>
    export default {
        name: 'SignUpAlertComponent'
    }
</script>