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

import { UsersRound,FilePenLine,FileDown,FileUp,File } from 'lucide-vue-next'


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
  <div class="flex flex-1 gap-4 p-5 rounded-lg border border-dashed shadow-sm grid grid-cols-8 bg-gray-50">
    <div class="grid grid-cols-subgrid gap-4 col-span-8">
      <div class="col-span-2">
        <Card class="drop-shadow-md">
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
      <div class="col-span-1">
        <Card class="drop-shadow-md">
          <CardHeader>
            <CardTitle>User Count</CardTitle>
            <CardDescription>This shows number of users</CardDescription>
          </CardHeader>
          <CardContent class="text-3xl font-bold flex items-center gap-4 justify-center">
            <UsersRound class="w-12 h-12"/>
            {{ user_count.user_count }}
          </CardContent>
        </Card>
      </div>
      <div class="col-span-5">
        <Card class="drop-shadow-md">
          <CardHeader>
            <CardTitle class="flex gap-4 items-center"><FilePenLine class="w-8 h-8"/>Latest Document Update</CardTitle>
            <CardDescription>This shows the latest update on a document</CardDescription>
          </CardHeader>
          <CardContent>
            <Card>
              <CardContent class="flex items-center mt-5 font-medium gap-4">
                <File class="w-6 h-6"/> Document Name
              </CardContent>
            </Card>
          </CardContent>
        </Card>
      </div>
    </div>
    <div class="col-span-4">
      <Card class="drop-shadow-md">
          <CardHeader>
            <CardTitle class="flex gap-4 items-center"><FileUp class="w-8 h-8"/>Latest Document Uploaded</CardTitle>
            <CardDescription>This shows the most recent uploaded document</CardDescription>
          </CardHeader>
          <CardContent>
            <Card>
              <CardContent class="flex items-center mt-5 font-medium gap-4">
                <File class="w-6 h-6"/> Document Name
              </CardContent>
            </Card>
          </CardContent>
        </Card>
    </div>
    <div class="col-span-4">
      <Card class="drop-shadow-md">
          <CardHeader>
            <CardTitle class="flex gap-4 items-center"><FileDown class="w-8 h-8"/>Latest Document Downloaded</CardTitle>
            <CardDescription>This shows the most recent downloaded document</CardDescription>
          </CardHeader>
          <CardContent>
            <Card>
              <CardContent class="flex items-center mt-5 font-medium gap-4">
                <File class="w-6 h-6"/> Document Name
              </CardContent>
            </Card>
          </CardContent>
        </Card>
    </div>
    <div class="col-span-8">
      <Card class="drop-shadow-md">
        <CardHeader>
          <CardTitle>Total Document Upload and Download</CardTitle>
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
              :show-x-axis="true"
            />
        </CardContent>
      </Card>
    </div>
  </div>
</template>