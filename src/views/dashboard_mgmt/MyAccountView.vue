<template>
    <div class="page-my-account" dir="rtl">
        <h1 class="title">حساب کاربری من</h1>
        <button @click = "logout()" class="button is-danger">خروج</button>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'MyAccount',
    methods: {
        logout() {
            axios.post("/api/v1/token/logout/")
            .then(response => {
                axios.defaults.headers.common["Authorization"] = ""
                this.$store.commit('removeToken')
                this.$router.push('/')
            })
            .catch(error=>{
                console.log(error);
                
            })
        }
    }
}
</script>