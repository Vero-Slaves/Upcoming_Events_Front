<script setup lang="ts">

import { useAuthStore } from '@/stores/authStore';
import { ref,type Ref } from 'vue';
import { useRoute, useRouter } from 'vue-router';
// import type { IAuthUser } from '@/models/IAuthUser';
import AuthService from '@/services/AuthService';
import type { IRegisterUser } from '@/models/IRegisterUser';
import axios from 'axios';


const store = useAuthStore()
const service = new AuthService()

const input_email: Ref<string> = ref('')
const input_password: Ref<string> = ref('')

const route = useRoute()
const router = useRouter()

const register = async () => {

  // store.user.email = input_email.value
  
  // const data: IRegisterUser = {
  //   email: input_email.value,
  //   password: input_password.value
  // }

  // const responseData = await service.register(data)

  // if (input_email.value == store.user.email) {
  //   store.user.isAuthenticated = responseData.isAuthenticated
  //   store.user.roles = responseData.roles
  //   const redirectPath = '/'
  //   router.push(redirectPath)
  // }

  try {
    const data: IRegisterUser = {
      email: input_email.value,
      password: input_password.value,
      roles: "users",
      isAuthenticated: true,
    }
    isLoading.value = true;
    
    const responseData = await axios.post('/api/register', data);
    store.user.isAuthenticated = responseData.data.isAuthenticated;
    store.user.roles = responseData.data.roles;
    const redirectPath = '/';
    router.push(redirectPath);
  } catch (error) {
    console.error('Error al registrar:', error);

  } finally {
    isLoading.value = false;
  }

}

 const username = ref('')
 const password = ref('')
 const isLoading = ref(false);


</script>


<template>
  <body>
      <div id="container">
          <div></div>
          <div class="form">
              <img src="/src/assets/img/layout_set_logo.png" alt="">
                  <form @submit.prevent="register()">
                      <label for="correoelectronico">Correo electrónico</label>
                      <input class="input-field" type="text" v-model="input_email" required>
                      <label for="contrasena">Contraseña</label>
                      <input class="=input-field" type="password" v-model="input_password" required>
                      <button type="submit" :disabled="isLoading" class="btn btn-primary btn-lg">
                          {{ isLoading ? 'Register in...' : 'REGISTRATE' }}
                      </button>
                  </form>
          </div>
                  
      </div>

  </body>
</template>
<style scoped lang="scss">

@import url('https://fonts.googleapis.com/css2?family=Raleway&display=swap');

body {
    max-width: 100vw;
}

#container {
  background-image: url(src/assets/img/fondo_registro4.png);
  background-size:cover;
  background-repeat: no-repeat;
  height: 100vh;
  padding: 15% 3% 3% 3%;
  
  
.form {
  background-color:  rgb(39, 161, 56, 0.75);
  background-position: center;
  height: 50vh;
  width: 26vw;
  border-radius: 1rem;
  text-align: center;
  color: #FFFFFF;
  font-family: 'Raleway', sans-serif;
  font-size: 25px;
  align-items: center;
  margin: 0 auto;
  
}

button {
  background-color: #006db0;
  color: #FFFFFF;
  font-family: 'Raleway', sans-serif;
  font-size: 18px;
  width: 28%;
  height: 40%;
  border-radius: 0.5rem;
  margin: 10%;
}

label {
  margin: 1%;
  font-size: 20px;
}

input {
  border-radius: 0.6rem;
  background-color: #FAFAF9;
  border-color: #ffffff;
  margin: 2%;
  width: 80%;
  height: 20%;
  font-size: 22px;
  text-align: center;
}

img {
  margin: 8%;
 
}
}


</style>