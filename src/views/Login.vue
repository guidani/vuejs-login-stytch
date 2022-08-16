<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const email = ref("");
const password = ref("");

const Login = async () => {
  if(!email.value || !password.value){
    return alert('Preencha todos os campos')
  }

  const res = await fetch('http://localhost:3333/login', {
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
      <h2>Login</h2>
      <p>Login or create an acoount to start using the app</p>
    </header>

    <form @submit.prevent="Login">
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

      <input type="submit" value="Login" />
    </form>
    <footer>
      <p>
        Não tem uma conta?

        <router-link to="/register">Register</router-link>
      </p>
    </footer>
  </main>
</template>
