<template>
    <nav>
        <router-link to="/">Home</router-link>
        <router-link to="/feed">Feed</router-link>
        <router-link to="/register">Register</router-link>
        <router-link to="/sign-in">Login</router-link>
        <button @click.prevent="handleSignOut" v-if="isLoggedIn">Sign Out</button>
    </nav>
    <router-view />
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { getAuth, onAuthStateChanged, signOut } from 'firebase/auth'

const router = useRouter()

const isLoggedIn = ref(false)

let auth = null
onMounted(() => {
    auth = getAuth()

    onAuthStateChanged(auth, (user) => {
        if (!user) {
            isLoggedIn.value = false
            return
        }

        isLoggedIn.value = true
    })
})

const handleSignOut = () => {
    signOut(auth).then(() => {
        router.push('/')
    })
}
</script>


<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}

nav a {
    display: inline-block;
    padding: .5em;
    cursor: pointer;
    text-decoration: none;
    font-weight: bold;
    color: #333;
}
</style>
