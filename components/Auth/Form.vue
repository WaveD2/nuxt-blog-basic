<template>
  <div>
    <div class="w-full flex justify-center">
      <div class="w-10 h-10">
        <IconLogo />
      </div>
    </div>
    <form action="" class="space-y-6 pt-5" @submit.prevent="handleSubmit">
      <template v-if="variant === 'LOGIN'">
        <UIInput
          label="Username"
          placeholder="@username"
          v-model="loginForm.username" />
        <UIInput
          label="Password"
          type="password"
          placeholder="*********"
          v-model="loginForm.password" />
      </template>
      <template v-else-if="variant === 'REGISTER'">
        <UIInput
          label="Username"
          placeholder="@username"
          v-model="registerForm.username" />
        <UIInput
          label="Password"
          type="password"
          placeholder="*********"
          v-model="registerForm.password" />
        <UIInput
          label="Confirm Password"
          type="password"
          placeholder="*********"
          v-model="registerForm.confirmpassword" />
        <UIInput
          label="Name"
          placeholder="Your Name"
          v-model="registerForm.name" />
        <UIInput
          label="Email"
          type="email"
          placeholder="example@example.com"
          v-model="registerForm.email" />
      </template>
      <div v-if="isLoading" class="flex justify-center">
        <IconLoading class="mx-auto my-5" />
      </div>
      <UIButton liquid :disabled="isDisabled">{{
        variant === "LOGIN" ? "Login" : "Register"
      }}</UIButton>

      <div class="flex gap-2 justify-center text-sm mt-6 px-2 text-gray-500">
        <div @click="toggleVariant">
          {{
            variant === "REGISTER"
              ? "New to Messenger?"
              : "Already have an account?"
          }}
        </div>
        <div @click="toggleVariant" class="underline cursor-pointer">
          {{ variant === "LOGIN" ? "Create an account" : "Login" }}
        </div>
      </div>
    </form>
  </div>
</template>

<script setup lang="ts">
  // API
  const {login, register} = useAuth();

  const loginForm = reactive({
    username: "",
    password: "",
  });

  const registerForm = reactive({
    username: "",
    password: "",
    confirmpassword: "",
    name: "",
    email: "",
  });

  type VARIANT = "LOGIN" | "REGISTER";
  const variant = ref<VARIANT>("LOGIN");

  const isLoading = ref(false);

  const handleSubmit = async () => {
    isLoading.value = true;
    try {
      if (variant.value === "LOGIN") {
        await login(loginForm);
      } else {
        await register(registerForm);
      }
    } catch (error) {
      console.log(error);
    } finally {
      isLoading.value = false;
    }
  };

  const isDisabled = computed(() => {
    if (variant.value === "LOGIN") {
      const {username, password} = loginForm;
      return !username || !password || isLoading.value;
    } else {
      const {username, password, confirmpassword, name, email} = registerForm;
      return (
        !username ||
        !password ||
        !confirmpassword ||
        !name ||
        !email ||
        isLoading.value
      );
    }
  });

  const toggleVariant = () => {
    if (variant.value === "REGISTER") {
      variant.value = "LOGIN";
    } else {
      variant.value = "REGISTER";
    }
  };
</script>
