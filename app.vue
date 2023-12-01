<template>
  <div>
    <NuxtLayout>
      <Header
        @filterByName="filterByName"
        @filterByType="filterByType"
        @filterByModality="filterByModality"
      />
      <InscriptionForm v-if="showRegister" @showRegister="setShowRegister" />
      <CoursesList
        :filteredCourses="filteredCourses"
        @showRegister="setShowRegister"
      />
    </NuxtLayout>
  </div>
</template>

<script setup lang="ts">
interface Course {
  name: string;
  type: string;
  modality: string;
}

const { data: courses } = useFetch<Course[]>("http://localhost:8000/courses", {
  transform: (_courses) => _courses,
});
const filteredCourses = ref<Array<Course>>([]);
const showRegister = ref(false);

const normalizeString = (str: string) => {
  return str.normalize("NFD").replace(/[\u0300-\u036f]/g, "");
};

const filterByName = (name: string) => {
  const normalizedFilter = normalizeString(name.trim().toLowerCase());
  filteredCourses.value = (courses.value || []).filter((course) =>
    normalizeString(course.name.toLowerCase()).includes(normalizedFilter)
  );
};

const setShowRegister = (value: boolean) => {
  showRegister.value = value;
};

const filterByType = (type: string) => {
  filteredCourses.value = (courses.value || []).filter(
    (course) => course.type === type
  );
};

const filterByModality = (modality: string) => {
  if (modality === "") {
    filteredCourses.value = courses.value || [];
  } else {
    filteredCourses.value = (courses.value || []).filter(
      (course) => course.modality === modality
    );
  }
};

onMounted(() => {
  filterByName("");
});
</script>
