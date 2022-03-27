<template>
  <div class="bg-gray-100 min-h-screen">
    <!-- This example requires Tailwind CSS v2.0+ -->
  <nav class="bg-green-500">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-16">
        <div class="flex items-center">
          <div class="flex-shrink-0">
            <img class="hidden lg:block h-10 w-auto" src="~/assets/logo.jpg">
          </div>
        </div>
        <span class="text-white font-medium text-xl cursor-pointer" @click="logout">Log Out</span>
      </div>
    </div>
  </nav>
    <Nuxt />
  </div>
</template>

<script>
export default {
  methods:{
    async logout() {
      await this.$auth.logout();
    },
  }
}
</script>
