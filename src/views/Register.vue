<script setup>
import { ref } from "vue";
import { useRouter } from 'vue-router'

const router = useRouter()

const email = ref("");
const password = ref("");
const confirmPassword = ref("");

const Register = async () => {
  if(!email.value || !password.value || !confirmPassword.value){
    return alert('Preencha todos os campos')
  }

  if(password.value !== confirmPassword.value){
    return alert('As senhas não são iguais!')
  }

  const res = await fetch('http://localhost:3333/register', {
    method: 'POST',
    headers: {
      'Content-Type' : 'application/json',
    },
    body: JSON.stringify({
      email: email.value,
      password: password.value,
    })
  }).then(res => res.json())

  if(res.success){
    localStorage.setItem('token', res.token)
    router.push('/')
  } else {
    alert(res.message)
  }
}

</script>

<template>
  <main>
    <header>
      <h1 class="logo">SECRETE APP</h1>
      <h2>REGISTER</h2>
      <p>create an acoount to start using the app</p>
    </header>

    <form @submit.prevent="Register">
      <div class="input__group">
        <label>
          <span>Enter your email</span>
          <input type="email" v-model="email" placeholder="teste@teste.com" />
        </label>
      </div>
      <div class="input__group">
        <label>
          <span>Enter your password</span>
          <input type="password" v-model="password" placeholder="***********" />
        </label>
      </div>
      <div class="input__group">
        <label>
          <span>Confirm your password</span>
          <input
            type="password"
            v-model="confirmPassword"
            placeholder="***********"
          />
        </label>
      </div>

      <input type="submit" value="Register" />
    </form>
    <footer>
      <p>
        Já tem uma conta?

        <router-link to="/login">Login</router-link>
      </p>
    </footer>
  </main>
</template>
