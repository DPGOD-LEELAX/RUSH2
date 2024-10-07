<script setup lang="ts">
import { CircleUser, Menu, Package2, LayoutDashboard, Folder, Building, FileUp } from 'lucide-vue-next'
import { Button } from '@/components/ui/button'
import { DropdownMenu, DropdownMenuContent, DropdownMenuItem, DropdownMenuLabel, DropdownMenuSeparator, DropdownMenuTrigger } from '@/components/ui/dropdown-menu'
import { Sheet, SheetContent, SheetTrigger } from '@/components/ui/sheet'
import { useRoute } from 'vue-router'
import { ref, watch, onMounted } from 'vue'

const { $jwtAuth } = useNuxtApp()
const { user, loggedIn } = useJwtAuth()

async function logout() {
  try {
    await $jwtAuth.logout()
  } catch (e) {
    // your error handling
  }
}

const route = useRoute()
const isIndexRoute = ref(false)
const isFMRoute = ref(false)

const onRouteChange = () => {
  isIndexRoute.value = route.path === '/'
  isFMRoute.value = route.path === '/file_manager'
}

watch(
  () => route.fullPath,
  () => {
    onRouteChange()
  }
)

onMounted(() => {
  onRouteChange()
})

</script>

<template>
  <div class="grid min-h-screen w-full md:grid-cols-[220px_1fr] lg:grid-cols-[280px_1fr]">
    <div class="hidden border-r md:block" style="background-color: #1F305E;">
      <div class="flex h-full max-h-screen flex-col gap-2">
        <div class="flex h-14 items-center border-b px-4 lg:h-[60px] lg:px-6">
          <a href="/" class="flex items-center gap-2 font-semibold">
            <Package2 class="h-6 w-6" />
            <p class="text-2xl text-white">MAYOYAO</p>
            <p class="text-2xl text-white">LGU</p>
          </a>
        </div>
        <div class="flex-1">
          <nav class="grid items-start px-2 text-lg font-medium lg:px-4" style="background-color: #1F305E;">
            <NuxtLink
              to="/"
              class="flex items-center gap-3 rounded-lg px-3 py-3 transition-all hover:text-primary"
              :class="{ 'bg-muted text-primary' : isIndexRoute, 'text-muted-foreground': !isIndexRoute, 'text-white': isIndexRoute }"
            >
              <LayoutDashboard class="h-5 w-5" :color="isIndexRoute ? '#1F305E' : 'white'"/>
              <span :class="{'text-white': !isIndexRoute, 'text-primary': isIndexRoute}">Dashboard</span>
            </NuxtLink>
            <NuxtLink
              to="/file_manager"
              class="flex items-center gap-3 rounded-lg px-3 py-3 transition-all hover:text-primary"
              :class="{ 'bg-muted text-primary' : isFMRoute, 'text-muted-foreground': !isFMRoute, 'text-white': isFMRoute }"
            >
              <Folder class="h-5 w-5" :color="isFMRoute ? '#1F305E' : 'white'"/>
              <span :class="{'text-white': !isFMRoute, 'text-primary': isFMRoute}">File Manager</span>
            </NuxtLink>
          </nav>
        </div>
      </div>
    </div>
    <div class="flex flex-col">
      <header class="flex h-14 items-center gap-4 border-b bg-muted/40 px-4 lg:h-[60px] lg:px-6">
        <Sheet>
          <SheetTrigger as-child>
            <Button
              variant="outline"
              size="icon"
              class="shrink-0 md:hidden"
            >
              <Menu class="h-5 w-5" />
              <span class="sr-only">Toggle navigation menu</span>
            </Button>
          </SheetTrigger>
          <SheetContent side="left" class="flex flex-col">
            <nav class="grid gap-2 text-lg font-medium" style="background-color: #1F305E;">
              <a
                href="/"
                class="flex items-center gap-3 rounded-lg px-3 py-3 bg-muted text-primary transition-all hover:text-primary"
              >
                <LayoutDashboard class="h-5 w-5" />
                Dashboard
              </a>
              <a
                href="#"
                class="flex items-center gap-3 rounded-lg px-3 py-3 text-muted-foreground transition-all hover:text-primary"
              >
                <Folder class="h-5 w-5" />
                File Manager
              </a>
              <a
                href="#"
                class="flex items-center gap-3 rounded-lg px-3 py-3 text-muted-foreground transition-all hover:text-primary"
              >
                <Building class="h-5 w-5" />
                Office Department
              </a>
              <a
                href="#"
                class="flex items-center gap-3 rounded-lg px-3 py-3 text-muted-foreground transition-all hover:text-primary"
              >
                <FileUp class="h-5 w-5" />
                Upload Document
              </a>
            </nav>
          </SheetContent>
        </Sheet>
        <div class="w-1/4 mr-auto">
          <p class="font-bold lg:text-xl md:text-md">Greetings!</p>
          <p class="lg:text-sm md:text-xs" v-if="loggedIn">{{ user.username }}</p>
        </div>
        <DropdownMenu>
          <DropdownMenuTrigger as-child>
            <Button variant="secondary" size="icon" class="rounded-full">
              <CircleUser class="h-5 w-5" />
              <span class="sr-only">Toggle user menu</span>
            </Button>
          </DropdownMenuTrigger>
          <DropdownMenuContent align="end">
            <DropdownMenuLabel>My Account</DropdownMenuLabel>
            <DropdownMenuSeparator />
            <DropdownMenuItem>Settings</DropdownMenuItem>
            <DropdownMenuItem>Support</DropdownMenuItem>
            <DropdownMenuSeparator />
            <DropdownMenuItem @click="logout">Logout</DropdownMenuItem>
          </DropdownMenuContent>
        </DropdownMenu>
      </header>
      <main class="flex flex-1 flex-col gap-4 p-4 lg:gap-6 lg:p-6">
        <slot />
      </main>
    </div>
  </div>
</template>
