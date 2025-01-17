<script setup lang="ts">
import { LoginForm } from "@/components/custom/login-form";
import { SignupForm } from "@/components/custom/signup-form";

const authType = ref("login");

const toggleAuthType = () => {
  authType.value = authType.value === "login" ? "signup" : "login";
};
</script>

<template>
  <div>
    <div class="min-h-screen flex flex-col justify-center items-center">
      <LoginForm v-if="authType == 'login'" @toggleAuth="toggleAuthType"/>
      <SignupForm v-if="authType == 'signup'" @toggleAuth="toggleAuthType" />
    </div>
  </div>
</template>
