<script setup lang="ts">
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";
import {
  Sidebar,
  SidebarContent,
  SidebarFooter,
  SidebarGroup,
  SidebarGroupLabel,
  SidebarHeader,
  SidebarInset,
  SidebarMenu,
  SidebarMenuAction,
  SidebarMenuButton,
  SidebarMenuItem,
  SidebarMenuSub,
  SidebarMenuSubButton,
  SidebarMenuSubItem,
  SidebarProvider,
  SidebarRail,
  SidebarTrigger,
} from "@/components/ui/sidebar";
import {
  BadgeCheck,
  Bell,
  Calendar1,
  CalendarDays,
  ChevronsUpDown,
  GraduationCap,
  Home,
  LogOut,
  Settings,
  Wallet,
} from "lucide-vue-next";

const user = useSupabaseUser();
const data = {
  navMain: [
    {
      title: "Calendar",
      url: "/calendar",
      icon: Calendar1,
    },
    {
      title: "Events",
      url: "/events",
      icon: CalendarDays,
    },
    {
      title: "Projects",
      url: "/projects",
      icon: GraduationCap,
    },
    {
      title: "Finances",
      url: "/finances",
      icon: Wallet,
    },
  ],
};
</script>

<template>
  <div>
    <SidebarProvider>
      <Sidebar collapsible="icon">
        <SidebarHeader>
          <SidebarMenu>
            <SidebarMenuItem>
              <SidebarMenuButton size="lg" asChild>
                <NuxtLink to="/">
                  <div
                    class="flex aspect-square size-8 items-center justify-center rounded-lg bg-sidebar-primary text-sidebar-primary-foreground"
                  >
                    <div class="w-[32px] h-[32px] bg-gray-500"></div>
                  </div>
                  <div class="flex flex-col gap-0.5 leading-none">
                    <span class="font-semibold">Clove</span>
                    <span class="">v0.1.0</span>
                  </div>
                </NuxtLink>
              </SidebarMenuButton>
            </SidebarMenuItem>
          </SidebarMenu>
        </SidebarHeader>
        <SidebarContent>
          <SidebarGroup>
            <SidebarMenu>
              <SidebarMenuItem>
                <SidebarMenuButton asChild tooltip="Home">
                  <NuxtLink to="/home">
                    <Home />
                    <span>Home</span>
                  </NuxtLink>
                </SidebarMenuButton>
              </SidebarMenuItem>
              <SidebarMenuItem>
                <SidebarMenuButton asChild tooltip="Notifications">
                  <NuxtLink to="/notifications">
                    <Bell />
                    <span>Notifications</span>
                  </NuxtLink>
                </SidebarMenuButton>
              </SidebarMenuItem>
            </SidebarMenu>
          </SidebarGroup>
          <SidebarGroup>
            <SidebarGroupLabel>Platform Apps</SidebarGroupLabel>
            <SidebarMenu>
              <SidebarMenuItem v-for="item in data.navMain" :key="item.title">
                <NuxtLink :to="item.url" as-child>
                  <SidebarMenuButton>
                    <component :is="item.icon" />
                    <span>{{ item.title }}</span>
                  </SidebarMenuButton>
                </NuxtLink>
              </SidebarMenuItem>
            </SidebarMenu>
          </SidebarGroup>
        </SidebarContent>
        <SidebarFooter>
          <SidebarMenu>
            <SidebarMenuItem>
              <DropdownMenu>
                <DropdownMenuTrigger as-child>
                  <SidebarMenuButton
                    size="lg"
                    class="data-[state=open]:bg-sidebar-accent data-[state=open]:text-sidebar-accent-foreground"
                  >
                    <Avatar class="h-8 w-8 rounded-lg">
                      <AvatarImage
                        :src="user?.avatar"
                        :alt="user?.user_metadata.name"
                      />
                      <AvatarFallback class="rounded-lg"> EC </AvatarFallback>
                    </Avatar>
                    <div class="grid flex-1 text-left text-sm leading-tight">
                      <span class="truncate font-semibold">{{
                        user?.user_metadata.name
                      }}</span>
                      <span class="truncate text-xs">{{ user?.email }}</span>
                    </div>
                    <ChevronsUpDown class="ml-auto size-4" />
                  </SidebarMenuButton>
                </DropdownMenuTrigger>
                <DropdownMenuContent
                  class="w-[--radix-dropdown-menu-trigger-width] min-w-56 rounded-lg"
                  side="bottom"
                  align="end"
                  :side-offset="4"
                >
                  <DropdownMenuLabel class="p-0 font-normal">
                    <div
                      class="flex items-center gap-2 px-1 py-1.5 text-left text-sm"
                    >
                      <Avatar class="h-8 w-8 rounded-lg">
                        <AvatarImage
                          :src="user?.avatar"
                          :alt="user?.user_metadata.name"
                        />
                        <AvatarFallback class="rounded-lg"> CN </AvatarFallback>
                      </Avatar>
                      <div class="grid flex-1 text-left text-sm leading-tight">
                        <span class="truncate font-semibold">{{
                          user?.user_metadata.name
                        }}</span>
                        <span class="truncate text-xs">{{ user?.email }}</span>
                      </div>
                    </div>
                  </DropdownMenuLabel>
                  <DropdownMenuSeparator />
                  <DropdownMenuGroup>
                    <DropdownMenuItem>
                      <BadgeCheck />
                      Account
                    </DropdownMenuItem>
                  </DropdownMenuGroup>
                  <DropdownMenuGroup>
                    <DropdownMenuItem>
                      <Settings />
                      Settings
                    </DropdownMenuItem>
                  </DropdownMenuGroup>
                  <DropdownMenuItem>
                    <LogOut />
                    Log out
                  </DropdownMenuItem>
                </DropdownMenuContent>
              </DropdownMenu>
            </SidebarMenuItem>
          </SidebarMenu>
        </SidebarFooter>
        <SidebarRail />
      </Sidebar>
      <SidebarInset>
        <slot />
      </SidebarInset>
    </SidebarProvider>
  </div>
</template>
