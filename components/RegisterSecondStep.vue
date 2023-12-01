<template>
  <div class="register-second-container">
    <button class="close-btn" @click="closeRegister">X</button>
    <form class="register-second-form" @submit.prevent="handleSubmit">
      <h2 class="register-second-title">Inscrição (1/3)</h2>
      <div class="register-second-wrapper">
        <label class="register-second-label" for="cep">CEP</label>
        <div class="register-second-input-wrapper">
          <input
            class="register-second-input"
            v-model="formData.cep"
            type="text"
            id="cep"
            name="cep"
            placeholder="Digite seu CEP"
          />
          <span
            class="register-second-error"
            v-for="error in v$.cep.$errors"
            :key="error.$uid"
          >
            {{ error.$message }}
          </span>
        </div>
      </div>
      <div class="register-second-wrapper">
        <label class="register-second-label" for="cpf">CPF</label>
        <div class="register-second-input-wrapper">
          <input
            class="register-second-input"
            v-model="formData.cpf"
            type="text"
            id="cpf"
            name="cpf"
            placeholder="Digite seu CPF"
          />
          <span
            class="register-second-error"
            v-for="error in v$.cpf.$errors"
            :key="error.$uid"
          >
            {{ error.$message }}
          </span>
        </div>
      </div>
      <div class="register-second-wrapper">
        <label class="register-second-label" for="birthdate"
          >Data de nascimento</label
        >
        <div class="register-second-input-wrapper">
          <input
            class="register-second-input"
            v-model="formData.birthdate"
            type="text"
            id="birthdate"
            name="birthdate"
            placeholder="Digite seu telefone"
          />
          <span
            class="register-second-error"
            v-for="error in v$.birthdate.$errors"
            :key="error.$uid"
          >
            {{ error.$message }}
          </span>
        </div>
      </div>
      <button class="register-second-btn" type="submit">Prosseguir</button>
    </form>
  </div>
</template>


<script setup lang="ts">

import { required, email, minLength, helpers } from "@vuelidate/validators";
import { useVuelidate } from "@vuelidate/core";
import { reactive } from "vue";

const props = defineProps(["userData"]);

const emit = defineEmits();

const formData = reactive({
  cep: "",
  cpf: "",
  birthdate: new Date(),
  courses: [],
});

const rules = computed(() => {
  return {
    cep: {
      required: helpers.withMessage("asdas", required),
      minLength: minLength(2),
    },
    cpf: { required, email },
    birthdate: { required },
  };
});

const v$ = useVuelidate(rules, formData);

const handleSubmit = async () => {
  const { data, status, error } = await useFetch(
    "http://localhost:8000/users/" + props.userData.id,
    {
      method: "PATCH",
      body: { ...formData },
      onRequestError: (error) => console.error(error),
    }
  );
  if (status.value === "success") {
    emit("toStep3", "3");
    emit("userData", data.value);
  }
};

const closeRegister = () => {
  emit("showRegister", false);
};
</script>


<style lang="sass">

.register-second-container
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

.register-second-form
  display: flex
  flex-direction: column
  padding: 3rem 2rem
  gap: 1.5rem
  border: 2px solid rgba(0, 0, 0, 0.1)
  border-radius: 8px

.register-second-title
  font-weight: 600
  text-align: center

.register-second-wrapper
  display: flex
  flex-direction: column
  gap: 0.5rem

.register-second-label
  font-weight: 600

.register-second-input-wrapper
  display: flex
  flex-direction: column

.register-second-input
  border: none
  background-color: rgba(0, 0, 0, 0.05)
  padding: 0.5rem 1rem
  border-radius: 5px

.register-second-error
  color: red
  font-size: 0.8rem

.register-second-btn
  padding: 0.5rem 1rem
  border: none
  background-color: #ABFD0E
  border-radius: 6px
  font-weight: 600

@media screen and (min-width: 720px)
  .register-second-container
    width: 40%
  .register-second-form
    width: 100%

</style>

