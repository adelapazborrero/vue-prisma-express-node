<script setup lang="ts">
import axios from 'axios';
import { ref, reactive } from 'vue'

const users = ref()

const getData = () => {
    axios.get('http://localhost:4444').then(res => users.value = res.data)
}

const userData = reactive({
    name: '',
    email: ''
})

const submitUser = () => {
    axios.post('http://localhost:4444/user', {
        name: userData.name,
        email: userData.email
    })
        .then(() => {
            userData.name = ''
            userData.email = ''
        }).then(() => getData())
}

const deleteUser = (userId: string) => {
    axios.delete('http://localhost:4444/user/' + userId).then(() => getData())
}

getData()

</script>

<template>
    <div
        style="margin: 15px; border: 1px solid black; padding: 10px; border-radius: 5px;"
        v-for="user in users"
        :key="user.id"
    >
        <p style="font-weight:bold;">{{ user.name }}</p>
        <p>{{ user.email }}</p>
        <button @click="deleteUser(user.id)">Delete</button>
    </div>
    <form @submit.prevent="submitUser">
        <input v-model="userData.name" />
        <input v-model="userData.email" />
        <button>Submit</button>
    </form>
</template>

<style>
@import "@/assets/base.css";

#app {
    max-width: 1280px;
    margin: 0 auto;
    padding: 2rem;

    font-weight: normal;
}

header {
    line-height: 1.5;
    max-height: 100vh;
}

.logo {
    display: block;
    margin: 0 auto 2rem;
}

a,
.green {
    text-decoration: none;
    color: hsla(160, 100%, 37%, 1);
    transition: 0.4s;
}

@media (hover: hover) {
    a:hover {
        background-color: hsla(160, 100%, 37%, 0.2);
    }
}

nav {
    width: 100%;
    font-size: 12px;
    text-align: center;
    margin-top: 2rem;
}

nav a.router-link-exact-active {
    color: var(--color-text);
}

nav a.router-link-exact-active:hover {
    background-color: transparent;
}

nav a {
    display: inline-block;
    padding: 0 1rem;
    border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
    border: 0;
}

@media (min-width: 1024px) {
    body {
        display: flex;
        place-items: center;
    }

    #app {
        display: grid;
        grid-template-columns: 1fr 1fr;
        padding: 0 2rem;
    }

    header {
        display: flex;
        place-items: center;
        padding-right: calc(var(--section-gap) / 2);
    }

    header .wrapper {
        display: flex;
        place-items: flex-start;
        flex-wrap: wrap;
    }

    .logo {
        margin: 0 2rem 0 0;
    }

    nav {
        text-align: left;
        margin-left: -1rem;
        font-size: 1rem;

        padding: 1rem 0;
        margin-top: 1rem;
    }
}
</style>
