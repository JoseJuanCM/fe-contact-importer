<template>
  <div class="py-6">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 md:px-8">
      <h1 class="text-2xl font-semibold text-gray-600">
        Welcome to Contact Import
      </h1>
    </div>
    <div class="max-w-7xl mx-auto px-4 sm:px-6 md:px-8">
      <!-- Replace with your content -->
      <div class="py-4">
        <div class="border-4 border-dashed border-gray-200 rounded-lg h-96 grid justify-items-center place-content-center">
          <NuxtLink to="/contacts" class="text-3xl font-medium text-gray-600 m-4">
            Contacts
          </NuxtLink>

          <NuxtLink to="/load" class="text-3xl font-medium text-gray-600 m-4">
            Load New File
          </NuxtLink>

          <NuxtLink to="/files" class="text-3xl font-medium text-gray-600 m-4">
            Previous Files
          </NuxtLink>
        </div>
      </div>
      <!-- /End replace -->
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  layout: 'base',
}
</script>
