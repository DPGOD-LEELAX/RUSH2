<script setup lang="ts">
import {ref, onMounted} from 'vue'


definePageMeta({
  middleware: 'auth',
  layout:'default'
})
const { $axios } = useNuxtApp()
const data = ref()



onMounted(async () => {
  try {
    const response = await $axios.get('user/')
    data.value = response.data
    console.log(response.data)
  } catch (err) {
    console.log(err)
  }
})
</script>
<template>
  <div class="flex items-center">
    <h1 class="text-lg font-semibold md:text-2xl">
      Dashboard
    </h1>
  </div>
  <div class="flex flex-1 items-center justify-center rounded-lg border border-dashed shadow-sm">
    <div class="flex flex-col items-center gap-1 text-center">
      <!-- {{ data.username }}  -->
    </div>
  </div>
</template>