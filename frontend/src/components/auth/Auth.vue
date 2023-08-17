<template>
    <div class="auth-content">
        <div class="auth-modal">
            <img src="../../assets/logo.png" width="350" alt="Logo">
            <hr>
            <div class="auth-title">{{ showSignup ? 'Cadastro' : 'Login' }}</div>

            <input v-if="showSignup" v-model="user.name" placeholder="Nome" type="text">
            <input v-model="user.email" placeholder="E-mail" type="text">
            <input v-model="user.password" placeholder="Senha" type="password">
            <input v-if="showSignup" v-model="user.confirmPassword" placeholder="Confirme a Senha" type="password">

            <button v-if="showSignup" @click="signup">Registrar</button>
            <button v-else @click="signin">Entrar</button>

            <a href @click.prevent="showSignup = !showSignup">
                <span v-if="showSignup">Já tem cadastro? Acesse o Login!</span>
                <span v-else>Não tem cadastro? Registre-se aqui!</span>
            </a>
        </div>
    </div>
</template>

<script>
import { baseApiUrl, showError, userKey } from '@/global'
import axios from 'axios'

export default {
    name: 'Auth',
    data: function () {
        return {
            showSignup: false,
            user: {}
        }
    },
    methods: {
        signin() {
            axios.post(`${baseApiUrl}/signin`, this.user)
                .then(res => {
                    this.$store.commit('setUser', res.data)
                    localStorage.setItem(userKey, JSON.stringify(res.data))
                    this.$router.push({ path: '/' })
                })
                .catch(showError)
        },
        signup() {
            axios.post(`${baseApiUrl}/signup`, this.user)
                .then(() => {
                    this.$toasted.global.defaultSuccess()
                    this.user = {}
                    this.showSignup = false
                })
                .catch(showError)
        }
    }
}
</script>

<style>
.auth-content {
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}

.auth-modal {
    background-color: #fff;
    width: 400px;
    padding: 35px;
    box-shadow: 0 1px 5px rgba(0, 0, 0, 0.2);
    border-radius: 5px;

    display: flex;
    flex-direction: column;
    align-items: center;
}

.auth-title {
    font-size: 1.2rem;
    font-weight: 100;
    margin-top: -30px;
    margin-bottom: 15px;
    color: #ff5e00;
}

.auth-modal input {
    border: 1px solid #BBB;
    width: 100%;
    margin-bottom: 15px;
    padding: 3px 8px;
    border-radius: 5px;
    outline: none;
}

.auth-modal input:hover {
    border: 1px solid #ff5e00;
    box-shadow: 0 1px 5px rgba(255, 106, 0, 0.458);

}

.auth-modal button {
    align-self: center;
    background-color: #ff7e00;
    color: #000;
    padding: 5px 15px;
    font-size: 1rem;
    font-weight: 500;
    border: 0.2px solid #BBB;
    border-radius: 5px;
}

.auth-modal button:hover {
    background-color: #ff6f00d1;
    color: #000;
}

.auth-modal a {
    margin-top: 35px;
    text-decoration: none;
    color: #6c6c6c;
}

.auth-modal a:hover {
    color: #ff5e00;
}
</style>