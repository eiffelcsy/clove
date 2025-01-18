<script setup lang="ts">
import { NuxtLink } from "#components";
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";

import {
  Breadcrumb,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbList,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from "@/components/ui/breadcrumb";
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/components/ui/card";
import { SidebarTrigger } from "@/components/ui/sidebar";
import { Separator } from "@/components/ui/separator";

import { CalendarArrowUp, CalendarDays, CalendarPlus2 } from "lucide-vue-next";
import CreateEventForm from "~/components/custom/create-event-form/CreateEventForm.vue";

definePageMeta({
  layout: "sidebar",
});
</script>

<template>
  <Head>
    <Title>Events - Clove</Title>
  </Head>
  <header
    class="flex h-16 shrink-0 items-center gap-2 transition-[width,height] ease-linear group-has-[[data-collapsible=icon]]/sidebar-wrapper:h-12"
  >
    <div class="flex items-center gap-2 px-4">
      <SidebarTrigger class="-ml-1" />
      <Separator orientation="vertical" class="mr-2 h-4" />
      <Breadcrumb>
        <BreadcrumbList>
          <BreadcrumbItem>
            <BreadcrumbLink as-child>
              <NuxtLink to="/home">Home</NuxtLink>
            </BreadcrumbLink>
          </BreadcrumbItem>
          <BreadcrumbSeparator />
          <BreadcrumbItem>
            <BreadcrumbPage>Events</BreadcrumbPage>
          </BreadcrumbItem>
        </BreadcrumbList>
      </Breadcrumb>
    </div>
  </header>
  <div class="flex flex-col lg:flex-row gap-4 p-4 pt-0 h-full">
    <div class="max-w-xl mx-auto flex flex-col gap-4">
      <Card>
        <CardHeader>
          <CardTitle class="flex flex-row items-center gap-2">
            <CalendarPlus2 class="size-[19px]" /> Join Event
          </CardTitle>
          <CardDescription>
            Join an event using shareable event code.
          </CardDescription>
        </CardHeader>
        <CardContent>
          <div class="flex w-full items-center space-x-2">
            <Input
              class="text-sm"
              type="email"
              placeholder="Enter Event Code"
            />
            <Button
              type="submit"
              class="bg-violet-500 hover:bg-violet-600 text-white"
            >
              Join Event
            </Button>
          </div>
        </CardContent>
      </Card>

      <CreateEventForm />

      <Card>
        <CardHeader>
          <CardTitle class="flex flex-row items-center gap-2">
            <CalendarArrowUp class="size-[19px]" /> Connect External Calendars
          </CardTitle>
          <CardDescription>
            Sync your external calendars to automatically import your
            availability & add confirmed events from Winter.
          </CardDescription>
        </CardHeader>
        <CardContent>
          <div class="flex flex-col gap-2">
            <Button class="bg-violet-500 hover:bg-violet-600 text-white">
              Connect to Google Calendar (Placeholder)
            </Button>
            <Button class="bg-violet-500 hover:bg-violet-600 text-white">
              Connect to Microsoft Calendar (Placeholder)
            </Button>
          </div>
        </CardContent>
      </Card>
    </div>
    <Card class="w-full bg-muted/50 border-none shadow-none lg:order-first">
      <CardHeader>
        <CardTitle class="flex flex-row items-center gap-2">
          <CalendarDays class="size-[19px]" /> Events Overview
        </CardTitle>
        <CardDescription>
          An overview of all past, current & upcoming events.
        </CardDescription>
      </CardHeader>
      <CardContent></CardContent>
    </Card>
  </div>
</template>
