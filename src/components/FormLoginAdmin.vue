<template>
  <div v-if="showLoginForm" class="overlay">
    <div class="login-form">
      <button class="close-button" @click="closeLoginForm">&times;</button>
      <h2>Login Admin</h2>
      <hr />
      <form @submit.prevent="handleSubmit">
        <div>
          <input
            type="text"
            id="username"
            v-model="username"
            required
            placeholder="Username"
          />
        </div>
        <div>
          <input
            type="password"
            id="password"
            v-model="password"
            required
            placeholder="Password"
          />
        </div>
        <div class="checkbox-item">
          <input type="checkbox" id="remember" v-model="remember" />
          <label for="remember">Remember Me</label>
        </div>
        <button type="submit">Login</button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios' // Pastikan axios sudah terpasang

export default {
  name: 'FormLoginAdmin',
  data() {
    return {
      username: '',
      password: '',
      remember: false,
      showLoginForm: true, // Mengontrol tampilan form login
    }
  },
  methods: {
    async handleSubmit() {
      try {
        const response = await axios.post('http://localhost:3000/api/login', {
          username: this.username,
          password: this.password,
        })

        console.log('Login berhasil:', response.data)
        this.$router.push('/dashboard') // Redirect ke halaman dashboard setelah berhasil login
      } catch (error) {
        console.error('Login gagal:', error.response.data)
        alert(error.response.data.error || 'Login gagal. Silakan coba lagi.') // Menampilkan pesan kesalahan
      }
    },
    closeLoginForm() {
      this.showLoginForm = false
      this.$router.push('/') // Redirect ke halaman utama jika form login ditutup
    },
  },
}
</script>

<style scoped>
/* Gaya CSS untuk form login tetap sama seperti sebelumnya */
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.login-form {
  background-color: whitesmoke;
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 50px 200px;
  border-radius: 20px;
  z-index: 1001;
  position: relative;
}

.close-button {
  background-color: hsla(160, 100%, 37%, 1);
  border: none;
  border-radius: 4px;
  color: black;
  cursor: pointer;
  font-size: 1.6em;
  width: 30px;
  position: absolute;
  top: 10px;
  right: 10px;
}

h2 {
  color: black;
  font-size: 32px;
  font-weight: bold;
}

hr {
  height: 2px;
  background-color: hsla(160, 100%, 37%, 1);
  border: none;
}

form {
  display: flex;
  flex-direction: column;
  gap: 10px;
  align-items: center;
}

form input {
  width: 400px;
  padding: 10px;
}

form .checkbox-item {
  display: block;
}

#remember {
  width: 30px;
}

label {
  color: black;
}

form button {
  padding: 15px;
  font-size: 14px;
  background-color: hsla(160, 100%, 37%, 1);
  border: none;
  border-radius: 4px;
}
</style>
