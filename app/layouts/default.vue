<template>
  <SidebarProvider class="overflow-y-hidden">
    <Sidebar/>
    <main>
      <slot />
    </main>
    <Toaster />
    <dialogs />
  </SidebarProvider>
</template>

<script setup>
import 'vue-sonner/style.css'
import { Toaster } from '~/components/ui/sonner'
import dialogs from '~/components/dialogs/dialogs.vue'
import { SidebarProvider } from '@/components/ui/sidebar'
import Sidebar from '~/layouts/includes/sidebar.vue'
</script>
