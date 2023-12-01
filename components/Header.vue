<template>
  <div class="nav-container">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container-fluid">
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarTogglerDemo03"
          aria-controls="navbarTogglerDemo03"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <a class="navbar-brand" href="/">
          <nuxt-img src="@/assets/img/logo-grupo.png"
                alt="Logo grupo integrado" class="logo" />
        </a>
        <div class="collapse navbar-collapse" id="navbarTogglerDemo03">
          <form class="d-flex" @submit.prevent="handleSubmit">
            <input
              v-model="searchInput"
              class="form-control me-2"
              type="search"
              placeholder="Digite o nome do curso"
              aria-label="Search"
            />
            <button class="btn btn-outline-success" type="submit">
              Procurar
            </button>
          </form>
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <a class="nav-link active" aria-current="page" href="/">Home</a>
            </li>
            <li
              @click="showAllCourses()"
              class="nav-item"
              type="button"
              data-bs-toggle="collapse"
              data-bs-target="#navbarTogglerDemo03"
              aria-controls="navbarTogglerDemo03"
              aria-expanded="false"
              aria-label="Toggle navigation"
            >
              Todos os cursos
            </li>
            <li
              @click="filterByType('Graduação')"
              class="nav-item"
              type="button"
              data-bs-toggle="collapse"
              data-bs-target="#navbarTogglerDemo03"
              aria-controls="navbarTogglerDemo03"
              aria-expanded="false"
              aria-label="Toggle navigation"
            >
              Graduação
            </li>
            <li
              @click="filterByType('Pós-graduação')"
              class="nav-item"
              type="button"
              data-bs-toggle="collapse"
              data-bs-target="#navbarTogglerDemo03"
              aria-controls="navbarTogglerDemo03"
              aria-expanded="false"
              aria-label="Toggle navigation"
            >
              Pós-graduação
            </li>
            <li
              @click="filterByModality('EAD')"
              class="nav-item"
              type="button"
              data-bs-toggle="collapse"
              data-bs-target="#navbarTogglerDemo03"
              aria-controls="navbarTogglerDemo03"
              aria-expanded="false"
              aria-label="Toggle navigation"
            >
              EAD
            </li>
            <li
              @click="filterByModality('Presencial')"
              class="nav-item"
              type="button"
              data-bs-toggle="collapse"
              data-bs-target="#navbarTogglerDemo03"
              aria-controls="navbarTogglerDemo03"
              aria-expanded="false"
              aria-label="Toggle navigation"
            >
              Presencial
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </div>
</template>

<script setup lang="ts">
const searchInput = ref("");
const emit = defineEmits();

const handleSubmit = () => {
  const filter = searchInput.value;
  emit("filterByName", filter);
};

const showAllCourses = () => {
  emit("filterByName", "");
};

const filterByType = (value: string) => {
  emit("filterByType", value);
};

const filterByModality = (value: string) => {
  emit("filterByModality", value);
};
</script>


<style lang="sass">
.d-flex
  padding: 1rem 0

.nav-container
  position: relative

.navbar
  position: sticky
  top: 0
  
</style>
