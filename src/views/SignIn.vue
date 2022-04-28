<template>
    <h1>Sign In to an Account</h1>
    <p>
        <input type="text" name="email" id="email" placeholder="Email" v-model="email">
    </p>
    <p>
        <input type="password" name="password" id="password" placeholder="Password" v-model="password">
    </p>
    <p v-if="errorMessage">{{ errorMessage }}</p>
    <p>
        <button @click="signIn">Submit</button>
    </p>
    <p>
        <button @click="signInWithGoogle">Sign In with Google</button>
    </p>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import {
    getAuth,
    signInWithEmailAndPassword,
    GoogleAuthProvider,
    signInWithPopup
} from 'firebase/auth'

const email = ref('')
const password = ref('')

const errorMessage = ref()

const router = useRouter()

const signIn = () => {
    const auth = getAuth()
    signInWithEmailAndPassword(auth, email.value, password.value)
        .then((data) => {
            console.log('Successfully signed!')
            console.log(auth.currentUser)
            router.push('/feed')
        })
        .catch(handleSignInErrorMessage)
}

const signInWithGoogle = () => {
    const provider = new GoogleAuthProvider()
    signInWithPopup(getAuth(), provider)
        .then((result) => {
            console.log(result.user)
            router.push('/feed')
        })
        .catch((error) => {

        })
}

function handleSignInErrorMessage (error) {
    switch (error.code) {
        case 'auth/invalid-email':
            errorMessage.value = 'Invalid email'
            break
        case 'auth/user-not-found':
            errorMessage.value = 'No account with that email was found'
            break
        case 'auth/wrong-password':
            errorMessage.value = 'Incorrect password'
            break
        default:
            errorMessage.value = 'Email or password was incorrect'
            break
    }
}
</script>