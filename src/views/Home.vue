<script setup>
    import { useRouter } from 'vue-router';

    const router = useRouter();

    const Logout = async () => {
        const res = await fetch('http://localhost:3333/logout', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                session_token: localStorage.getItem('token')
            })
        }).then( res => res.json())

        if (res.success){
            localStorage.removeItem('token')
            router.push('/login')
        } else {
            alert(res.message)
        }
    }
</script>

<template>
    <main>
        <h1>Home</h1>
        <button @click="Logout">logout</button>
    </main>
</template>