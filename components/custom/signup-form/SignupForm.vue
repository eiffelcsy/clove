<template>
  <div class="w-full flex items-center justify-center py-12">
    <div class="mx-auto grid w-[350px] gap-6">
      <div class="grid gap-2 text-center">
        <h1 class="text-3xl font-bold">Sign Up</h1>
        <p class="text-balance text-muted-foreground">
          Enter your details below to create a new account.
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
          <Label for="password">Password</Label>
          <Input
            id="password"
            type="password"
            autocomplete="new-password"
            v-model="password"
            required
          />
        </div>

        <div class="grid gap-2">
          <Label for="confirm-password">Confirm Password</Label>
          <Input
            id="confirm-password"
            type="password"
            autocomplete="new-password"
            v-model="confirmPassword"
            required
          />
        </div>

        <Button type="submit" class="w-full" @click="handleSignup">
          Sign Up
        </Button>
        <Button variant="outline" class="w-full"> Sign Up with Google </Button>
      </div>
      <div class="mt-4 text-center text-sm">
        Already have an account?
        <Button
          variant="link"
          @click="
            () => {
              emit('toggleAuth');
            }
          "
          class="px-0"
        >
          Login
        </Button>
      </div>
    </div>
    <Toaster />
  </div>
</template>

<script lang="ts" setup>
import { ref, h, defineEmits } from "vue";

import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import { Toaster, ToastAction } from "@/components/ui/toast";
import { useToast } from "@/components/ui/toast/use-toast";

const email = ref("");
const password = ref("");
const confirmPassword = ref("");

const supabase = useSupabaseClient();
const { toast } = useToast();

const emit = defineEmits(["toggleAuth"]);

const handleSignup = async () => {
  if (password.value !== confirmPassword.value) {
    toast({
      title: "Password Mismatch",
      description: "The passwords you entered do not match.",
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
    return;
  }

  const { data, error } = await supabase.auth.signUp({
    email: email.value,
    password: password.value,
  });

  if (error) {
    toast({
      title: "Sign Up Failed",
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
      title: "Sign Up Successful",
      description: "Please check your email to confirm your account.",
      variant: "success",
      duration: 1500,
    });
    setTimeout(() => {
      navigateTo("/login");
    }, 1700);
  }
};
</script>

<style scoped></style>
