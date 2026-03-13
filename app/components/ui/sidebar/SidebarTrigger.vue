<script setup lang="ts">
import type { HTMLAttributes } from "vue"
import { cn } from "@/lib/utils"
import { Button } from '@/components/ui/button'
import { useSidebar } from "./utils"
import { SidebarOpenIcon, SidebarCloseIcon } from 'lucide-vue-next'

const props = defineProps<{
  class?: HTMLAttributes["class"]
}>()

const { toggleSidebar, open } = useSidebar()
</script>

<template>
  <Button
    data-sidebar="trigger"
    data-slot="sidebar-trigger"
    variant="ghost"
    size="icon"
    :class="cn('h-8 w-8', props.class)"
    @click="toggleSidebar"
  >
    <SidebarOpenIcon v-if="open" />
    <SidebarCloseIcon v-else />
    <span class="sr-only">Toggle Sidebar</span>
  </Button>
</template>
