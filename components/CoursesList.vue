<script setup lang="ts">
const props = defineProps(["filteredCourses"]);

const emit = defineEmits();
const showRegister = () => {
  emit("showRegister", true)
}
</script>

<template>
  <div class="course-list">
    <ul v-if="filteredCourses && filteredCourses.length > 0">
      <CourseCard
        v-for="course in filteredCourses"
        :key="course.id"
        :course="course"
        @showRegister="showRegister"
      />
    </ul>
    <p v-else>Curso não encontrado</p>
  </div>
</template>

<style lang="sass">

.course-list
  background-color: #f0f0f0
  display: flex
  padding: 0 1rem
  justify-content: center
  flex-direction: column

ul
  display: flex
  // flex-direction: column
  flex-wrap: wrap
  width: 100%
  align-items: center
  justify-content: center
  gap: 1rem

p
  margin-top: 1rem
  text-align: center

</style>
