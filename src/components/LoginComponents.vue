<!-- template - основной шаблон где мы пишем HTML -->
<template>
    <form @submit.prevent="loginFunc">
        <div style="text-align: center;">
            <button id="login" @click.prevent="loginClick">login</button>
            <button id="reg" @click.prevent="regClick">registration</button>
        </div>
        <input id="mail" type="mail" v-model="mail" placeholder="mail">
        <input id="pass" type="password" v-model="pass" placeholder="pass">
        <input v-if="action == 'reg'" id="pass2" type="password" v-model="pass2" placeholder="retype pass">
        <br>
        <input id="submit" type="submit" @click.prevent="loginFunc" :value="submitVal">
    </form>
</template>

<script setup lang ="ts">
import md5 from 'md5'
import axios from 'axios'
import { ref } from 'vue'



const mail = ref('')
const pass = ref('')
const pass2 = ref('')
const action = ref('login')
const submitVal = ref('Log In')


const loginClick = () => {
    action.value = 'login'
    actionChange()
}

const regClick = () => {
    action.value = 'reg'
    actionChange()
}

function actionChange() {
    if (action.value == 'login') {
        submitVal.value = 'Log In'
    } else {
        submitVal.value = 'Register'
    }

}
async function loginFunc() {
    if (!mail.value) {
        alert('input mail')
        return
    }
    if (!pass.value) {
        alert('input pass')
        return
    }
   
    try {
        if (action.value == 'login') {
            const data = await axios.post('/login', {
                email: mail.value,
                hash: md5(pass.value).toString()
            })

            if (!data.data) {
                alert('check mail and pass')
                return
            }
            console.log(data)
            localStorage.id = data.data.id
            localStorage.uuid = data.data.uuid
            // location.assign('/')
        } else {
            if (pass.value != pass2.value) {
                alert('pass not eqals')
                return
            }

            const data = await axios.post('/create_user', {
                email: mail.value,
                hash: md5(pass.value).toString()
            })
            console.log(data)

            localStorage.id = data.data.id
            localStorage.uuid = data.data.uuid
            // location.assign('/')
        }
    } catch (e) {
        console.log(e)
    }
}
</script>
<!-- "Sсoped" - Если к стилям добавить "sсoped" они будут использованы только для этого компонента -->

<style scoped></style>