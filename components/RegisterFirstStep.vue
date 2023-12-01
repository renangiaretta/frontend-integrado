<template>
  <div class="register-first-container">
    <button class="close-btn" @click="closeRegister">X</button>
    <form class="register-first-form" @submit.prevent="handleSubmit">
      <h2 class="register-first-title">Inscrição (1/3)</h2>
      <div class="register-first-wrapper">
        <label class="register-first-label" for="name">Nome</label>
        <div class="register-first-input-wrapper">
          <input
            class="register-first-input"
            v-model="formData.name"
            type="text"
            id="name"
            name="name"
            placeholder="Digite seu nome"
          />
          <span
            class="register-first-error"
            v-for="error in v$.name.$errors"
            :key="error.$uid"
          >
            {{ error.$message }}
          </span>
        </div>
      </div>
      <div class="register-first-wrapper">
        <label class="register-first-label" for="email">E-mail</label>
        <div class="register-first-input-wrapper">
          <input
            class="register-first-input"
            v-model="formData.email"
            type="text"
            id="email"
            name="email"
            placeholder="Digite seu e-mail"
          />
          <span
            class="register-first-error"
            v-for="error in v$.email.$errors"
            :key="error.$uid"
          >
            {{ error.$message }}
          </span>
        </div>
      </div>
      <div class="register-first-wrapper">
        <label class="register-first-label" for="phone">Telefone</label>
        <div class="register-first-input-wrapper">
          <input
            class="register-first-input"
            v-model="formData.phone"
            type="text"
            id="phone"
            name="phone"
            placeholder="Digite seu telefone"
          />
          <span
            class="register-first-error"
            v-for="error in v$.phone.$errors"
            :key="error.$uid"
          >
            {{ error.$message }}
          </span>
        </div>
      </div>
      <button class="register-first-btn" type="submit">Prosseguir</button>
    </form>
  </div>
</template>

<script setup lang="ts">
import { required, email, minLength, helpers } from "@vuelidate/validators";
import useVuelidate from "@vuelidate/core";
import { reactive } from "vue";

const emit = defineEmits();

const formData = reactive({
  name: "",
  email: "",
  phone: "",
});

const closeRegister = () => {
  emit("showRegister", false);
};

const rules = {
  name: {
    required: helpers.withMessage("O nome deve ser preenchido", required),
    minLength: helpers.withMessage(
      "O nome deve ter no mínimo 2 caracteres",
      minLength(4)
    ),
  },
  email: {
    required: helpers.withMessage("O e-mail deve ser preenchido", email),
  },
  phone: {
    required: helpers.withMessage("O telefone deve ser preenchido", required),
  },
};

const v$ = useVuelidate(rules, formData);

const handleSubmit = async () => {
  const result = await v$.value.$validate();
  const { data, status, error } = await useFetch(
    "http://localhost:8000/users",
    {
      method: "POST",
      body: { ...formData },
      onRequestError: (error) => console.error(error),
    }
  );
  if (error.value) {
    console.error(error.value.data.message);
  }
  if (status.value === "success") {
    emit("toStep2", "2");
    emit("newUser", data.value);
  }
};

export interface IUser {
  name: string;
  email: string;
  phone: string;
}
</script>

<style lang="sass">

.register-first-container
  display: flex
  background-color: #FFFFFF
  width: 80%
  min-height: 80%
  max-height: 95%
  padding: 2.5rem
  justify-content: center
  align-items: center
  position: relative
  border-radius: 8px
  z-index: 20

.close-btn
  display: flex
  position: absolute
  top: 0.5rem
  right: 0.5rem
  justify-content: center
  align-items: center
  padding: 0.2rem 0.5rem
  border: 2px solid rgba(0, 0, 0, 0.2)
  background-color: inherit
  color: rgba(0, 0, 0, 0.4)
  border-radius: 8px
  font-size: 0.7rem

.register-first-form
  display: flex
  flex-direction: column
  padding: 3rem 2rem
  gap: 1.5rem
  border: 2px solid rgba(0, 0, 0, 0.1)
  border-radius: 8px

.register-first-title
  font-weight: 600
  text-align: center

.register-first-wrapper
  display: flex
  flex-direction: column
  gap: 0.5rem

.register-first-label
  font-weight: 600

.register-first-input-wrapper
  display: flex
  flex-direction: column

.register-first-input
  border: none
  background-color: rgba(0, 0, 0, 0.05)
  padding: 0.5rem 1rem
  border-radius: 5px

.register-first-error
  color: red
  font-size: 0.8rem

.register-first-btn
  padding: 0.5rem 1rem
  border: none
  background-color: #ABFD0E
  border-radius: 6px
  font-weight: 600

@media screen and (min-width: 720px)
  .register-first-container
    width: 40%
  .register-first-form
    width: 100%
</style>
