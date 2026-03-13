<template>
  <Sidebar collapsible="icon" class="overflow-hidden">
    <SidebarHeader class="flex flex-row items-center">
      <SidebarTrigger/>
      <span class="text-xl text-nowrap">Notes app</span>
    </SidebarHeader>
    <SidebarContent></SidebarContent>
    <SidebarFooter>
      <SidebarMenu>
        <SidebarMenuItem>
          <SidebarMenuButton>
            <Avatar>
              <AvatarImage :src="avatarUrl" alt="User avatar" />
              <AvatarFallback>{{ initials }}</AvatarFallback>
            </Avatar>
            <div class="text-left">
              <p class="text-sm font-medium">{{ username }}</p>
              <p class="text-sm text-muted-foreground">{{ email }}</p>
            </div>
          </SidebarMenuButton>
        </SidebarMenuItem>
      </SidebarMenu>
    </SidebarFooter>
  </Sidebar>
</template>

<script setup>
import { useUser } from '~/composables/useUser'
import { Sidebar, SidebarContent, SidebarFooter, SidebarHeader, SidebarTrigger } from '~/components/ui/sidebar'
import { Avatar, AvatarImage, AvatarFallback } from '~/components/ui/avatar'
const user = useUser()

const username = computed(() => user.user.value?.username ?? 'Guest')
const email = computed(() => user.user.value?.email ?? 'guest@example.com')

const initials = computed(() => {
  return username.value
    .split(' ')
    .map((w) => w[0].toUpperCase())
    .join('')
})

const avatarUrl =
  'https://hille-lebensmittel.de/wp-content/uploads/2016/09/platzhalter-portrait-750x750.jpg'
</script>
