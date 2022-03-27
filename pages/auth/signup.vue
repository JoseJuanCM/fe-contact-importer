<template>
  <div class="min-h-screenbg-gray-50 flex flex-col justify-center py-12 sm:px-6 lg:px-8">
    <div class="sm:mx-auto sm:w-full sm:max-w-md h-24 py-6">
      <img class="mx-auto h-14 w-auto" src="~/assets/logo.jpg" />
    </div>

    <div class="sm:mx-auto sm:w-full sm:max-w-md">
      <div class="bg-white py-8 px-4 shadow sm:rounded-lg sm:px-10">
        <form class="space-y-6" @submit.prevent="createUser">
          <div>
            <h2 class="text-center text-3xl font-extrabold text-gray-500">
              Sign Up
            </h2>
          </div>
          <div>
            <p v-for="error in errorMsg" :key="error" class="text-red-600 text-sm leading-tight 
              text-center">
              {{ error }}
            </p>
          </div>
          <div>
            <label for="email" class="block text-sm font-medium text-gray-600">
              Email
            </label>
            <div class="mt-1">
              <input id="email" name="email" type="email" autocomplete="email" required 
                class=" appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md 
                  shadow-sm placeholder-gray-400 focus:outline-none focus:ring-green-500 
                  focus:border-green-500 sm:text-sm " v-model="login.username" @focus="errorMsg = []"/>
            </div>
          </div>

          <div>
            <label for="password" class="block text-sm font-medium text-gray-600">
              Password
            </label>
            <div class="mt-1">
              <input id="password" name="password" type="password" autocomplete="current-password" 
                required class=" appearance-none block w-full px-3
                  py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 
                  focus:outline-none focus:ring-green-500 focus:border-green-500 sm:text-sm "
                v-model="login.password" @focus="errorMsg = []" />
            </div>
          </div>

          <div>
            <button type="submit" class=" w-full flex justify-center py-2 px-4 border 
              border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-green-400 
              hover:bg-green-500 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500">
              Sign up
            </button>
          </div>
        </form>

        <div class="mt-6">
          <div class="relative">
            <div class="absolute inset-0 flex items-center">
              <div class="w-full border-t border-gray-300"></div>
            </div>
            <div class="relative flex justify-center text-sm">
              <span class="px-2 bg-white text-gray-500"> Or Sign In </span>
            </div>
          </div>
        </div>

        <div class="mt-6">
          <NuxtLink to="/auth/login" class=" w-full flex justify-center py-2 px-4 border 
              border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-green-400 
              hover:bg-green-500 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500">
              Log in
          </NuxtLink>
          </div>

      </div>
    </div>
  </div>
</template>

<script>
export default {
  auth: false,
  data() {
    return {
      login: {
        username: "",
        password: "",
      },
      errorMsg: "",
    };
  },
  methods: {
    createUser(){
      const data = {
        "email": this.login.username,
        "password": this.login.password
        }
      
      this.$axios.post(`users/`,data)
      .then((response) => {
        if(response.status == 201){
          this.$router.push('/auth/login')
        }
      })
      .catch((error) => {
          this.errorMsg = error.response.data
      })

    }
  },
};
</script>
