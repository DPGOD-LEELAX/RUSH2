<script setup lang="ts">
import {ref, onMounted} from 'vue'
import { DonutChart } from '@/components/ui/chart-donut'
import { BarChart } from '@/components/ui/chart-bar'

import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'


const data = [
  { name: 'Jan', downloads: Math.floor(Math.random() * 5000) + 1000, uploads: Math.floor(Math.random() * 3000) + 500 },
  { name: 'Feb', downloads: Math.floor(Math.random() * 5000) + 1000, uploads: Math.floor(Math.random() * 3000) + 500 },
  { name: 'Mar', downloads: Math.floor(Math.random() * 5000) + 1000, uploads: Math.floor(Math.random() * 3000) + 500 },
  { name: 'Apr', downloads: Math.floor(Math.random() * 5000) + 1000, uploads: Math.floor(Math.random() * 3000) + 500 },
  { name: 'May', downloads: Math.floor(Math.random() * 5000) + 1000, uploads: Math.floor(Math.random() * 3000) + 500 },
  { name: 'Jun', downloads: Math.floor(Math.random() * 5000) + 1000, uploads: Math.floor(Math.random() * 3000) + 500 },
  { name: 'Jul', downloads: Math.floor(Math.random() * 5000) + 1000, uploads: Math.floor(Math.random() * 3000) + 500 },
  { name: 'Aug', downloads: Math.floor(Math.random() * 5000) + 1000, uploads: Math.floor(Math.random() * 3000) + 500 },
  { name: 'Sep', downloads: Math.floor(Math.random() * 5000) + 1000, uploads: Math.floor(Math.random() * 3000) + 500 },
  { name: 'Oct', downloads: Math.floor(Math.random() * 5000) + 1000, uploads: Math.floor(Math.random() * 3000) + 500 },
  { name: 'Nov', downloads: Math.floor(Math.random() * 5000) + 1000, uploads: Math.floor(Math.random() * 3000) + 500 },
  { name: 'Dec', downloads: Math.floor(Math.random() * 5000) + 1000, uploads: Math.floor(Math.random() * 3000) + 500 },
]


definePageMeta({
  middleware: 'auth',
  layout:'default'
})
const { $axios } = useNuxtApp()
const user_count = ref({
  user_count: 0
})
const document_status_count = ref({
  total_docuents: 0,
  status_counts: []
})

onMounted(async () => {
  await getUserCount()
  await getDocumentStatusCount()
  console.log(data)
})

const getUserCount = async () => {
  try {
    const response = await $axios.get('user-count/')
    user_count.value = response.data
    console.log(response.data)
  } catch (err) {
    console.log(err)
  }
}
const getDocumentStatusCount= async () => {
  try {
    const response = await $axios.get('get-document-count/')
    document_status_count.value = response.data
    console.log(document_status_count.value)
  } catch (err) {
    console.log(err)
  }
}
</script>
<template>
  <div class="flex items-center">
    <h1 class="text-lg font-semibold md:text-2xl">
      Dashboard
    </h1>
  </div>
  <div class="flex flex-1 gap-4 p-5 rounded-lg border border-dashed shadow-sm">
    <div class="">
      <Card>
        <CardHeader>
          <CardTitle>Document Count</CardTitle>
          <CardDescription>This shows document count based on status</CardDescription>
        </CardHeader>
        <CardContent>
          <DonutChart
            index="name"
            :category="'count'"
            :data="document_status_count.status_counts"
            :colors="['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'purple']"
          />
        </CardContent>
      </Card>
    </div>
    <div class="">
      <Card>
        <CardHeader>
          <CardTitle>User Count</CardTitle>
          <CardDescription>This shows number of users</CardDescription>
        </CardHeader>
        <CardContent>
          {{ user_count.user_count }}
        </CardContent>
      </Card>
    </div>
    <div>
      <Card class="w-[600px]">
        <CardHeader>
          <CardTitle>Document Uploads and Downloads</CardTitle>
          <CardDescription>This shows number of uploads and downloads on a monthly basis</CardDescription>
        </CardHeader>
        <CardContent class="flex justify-center">
            <BarChart
              :data="data"
              index="name"
              :categories="['downloads', 'uploads']"
              :y-formatter="(tick, i) => {
                return typeof tick === 'number'
                  ? `${new Intl.NumberFormat('us').format(tick).toString()}`
                  : ''
              }"
              :rounded-corners="4"
              :colors="['red','blue']"
            />
        </CardContent>
      </Card>
    </div>
  </div>
</template>