<script setup>
import { reactive, computed } from "vue";
import "./assets/main.scss";
import useVuelidate from "@vuelidate/core";
import {
  required,
  minLength,
  maxLength,
  email,
  sameAs,
} from "@vuelidate/validators";

const formData = reactive({
  name: "",
  email: "",
  password: "",
  passwordConfirm: "",
});

const rules = computed(() => {
  return {
    name: { required, minLength: minLength(3), maxLength: maxLength(20) },
    email: { required, email },
    password: { required, minLength: minLength(3), maxLength: maxLength(20) },
    passwordConfirm: { sameAs: sameAs(formData.password) },
  };
});

const v$ = useVuelidate(rules, formData);

const submitForm = async () => {
  const result = await v$.value.$validate();
  if (result) {
    alert("Вы успешно зарегистрированы");
  } else {
    alert("Регистрация не удалась");
  }
};
</script>

<template>
  <form @submit.prevent="submitForm" novalidate>
    <div class="step">
      <h2>Регистрация</h2>

      <input
        v-model="formData.name"
        type="text"
        placeholder="Имя"
        class="form-control"
        id="name"
      />

      <span v-for="error in v$.name.$errors" :key="error.$uid" class="text-red">
        {{ "Минимум 3 символа, максимум 20" }}
      </span>

      <input
        v-model="formData.email"
        type="text"
        placeholder="Почта"
        class="form-control"
        id="email"
      />

      <span
        v-for="error in v$.email.$errors"
        :key="error.$uid"
        class="text-red"
      >
        {{ "Некорректная почта" }}</span
      >

      <input
        v-model="formData.password"
        type="password"
        placeholder="Пароль"
        class="form-control"
        id="password"
      />

      <span
        v-for="error in v$.password.$errors"
        :key="error.$uid"
        class="text-red"
      >
        {{ "Минимум 3 символа, максимум 20" }}</span
      >

      <input
        v-model="formData.passwordConfirm"
        type="password"
        placeholder="Подтвердите пароль"
        class="form-control"
        id="passwordConfirm"
      />

      <span
        v-for="error in v$.passwordConfirm.$errors"
        :key="error.$uid"
        class="text-red"
      >
        {{ "Пароли не совпадают" }}</span
      >

      <button type="submit">Регистрация</button>
    </div>
  </form>
</template>

<script>
export default {
  validations() {
    return {};
  },
};
</script>
