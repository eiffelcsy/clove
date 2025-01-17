<template>
  <div class="w-full flex items-center justify-center py-12">
    <div class="mx-auto grid w-[350px] gap-6">
      <div class="grid gap-2 text-center">
        <h1 class="text-3xl font-bold">Login</h1>
        <p class="text-balance text-muted-foreground">
          Enter your email below to login to your account
        </p>
      </div>
      <div class="grid gap-4">
        <div class="grid gap-2">
          <Label for="email">Email</Label>
          <Input
            id="email"
            type="email"
            placeholder="m@example.com"
            autocomplete="email"
            v-model="email"
            required
          />
        </div>
        <div class="grid gap-2">
          <div class="flex items-center">
            <Label for="password">Password</Label>
            <a
              href="/forgot-password"
              class="ml-auto inline-block text-sm underline"
            >
              Forgot your password?
            </a>
          </div>
          <Input
            id="password"
            type="password"
            autocomplete="current-password"
            v-model="password"
            required
          />
        </div>
        <Button type="submit" class="w-full" @click="handleLogin">
          Login
        </Button>
        <Button variant="outline" class="w-full"> Login with Google </Button>
      </div>
      <div class="mt-4 text-center text-sm">
        Don't have an account?
        <Button
          variant="link"
          @click="
            () => {
              emit('toggleAuth');
            }
          "
          class="px-0"
        >
          Sign up
        </Button>
      </div>
    </div>
    <Toaster />
  </div>
</template>

<script lang="ts" setup>
import { ref, defineEmits } from "vue";

import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import { Toaster, ToastAction } from "@/components/ui/toast";
import { useToast } from "@/components/ui/toast/use-toast";

const email = ref("");
const password = ref("");

const supabase = useSupabaseClient();
const { toast } = useToast();

const emit = defineEmits(["toggleAuth"]);

const handleLogin = async () => {
  const { error } = await supabase.auth.signInWithPassword({
    email: email.value,
    password: password.value,
  });
  if (error) {
    toast({
      title: "Login Failed",
      description: error.message,
      variant: "destructive",
      action: h(
        ToastAction,
        {
          altText: "Try again",
        },
        {
          default: () => "Try again",
        }
      ),
    });
  } else {
    toast({
      title: "Login Successful",
      description: "You have successfully logged in.",
      variant: "success",
      duration: 1000,
    });
    setTimeout(() => {
      navigateTo("/home");
    }, 1200);
  }
};
</script>

<style></style>
