<script setup>
import {ref} from "vue";
import {useRouter} from "vue-router";
import useAuth from "../composable/useAuth";
import useError from "../composable/useError";

const { isAuthenticated, login, signup } = useAuth();

const username = ref("");
const password = ref("");

const router = useRouter();

const loginIn = async () => {
    await login(username.value, password.value);
    goToHome();
};

const signingUp = async () => {
    await signup(username.value, password.value);
    goToHome();
};

const goToHome = () => {
    if(isAuthenticated.value){
        router.push("/");
    }
    else{
        setError("Invalid username or password");
        start();
    }
}

const {error, setError} = useError();

import { useTimeout, promiseTimeout } from '@vueuse/core';

const { ready, start } = useTimeout(4000, { controls: true }, false);

</script>

<template >
<div class="flex-col space-y-12 flex items-center justify-center min-h-screen-nonav">
    <div class="bg-gray-200 flex justify-center items-center shadow-2xl overflow-hidden rounded-lg">
        <img class="h-64" src="../assets/bg_login.png" alt="Login BG">
        <form @submit.prevent="loginIn" class="flex flex-col space-y-4 p-4">
            <input type="text" class="p-2 border-2 rounded-lg" placeholder="Username" 
                v-model="username"/>
            <input type="password" class="p-2 border-2 rounded-lg" placeholder="Password" 
                v-model="password"/>
            <div class="flex space-x-2">
                <button @submit.prevent="loginIn" type="submit" class="w-1/2 py-2 rounded-lg bg-green-400 text-green-50">Login</button>
                <button @click="signingUp" class="w-1/2 py-2 rounded-lg bg-blue-400 text-blue-50">Sign Up</button>
            </div>
        </form>
    </div>
    <div v-if="!ready && error"
     class="w-1/4 absolute bottom-2 right-2 rounded-lg text-center p-4 bg-red-300 text-red-700 font-semibold border-2 border-red-700">{{error}}</div>
</div>
</template>