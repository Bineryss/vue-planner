<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
  </div>
</template>

<script>

import {onMounted, ref} from "vue";
import SingleProject from "@/components/SingleProject";

export default {
  name: 'Home',
  components: {SingleProject},
  setup() {
    const projects = ref([])

    const load = async () => {
      try {
        const data = await fetch("http://localhost:3000/projects")
        projects.value = await data.json()
      } catch (err) {
        console.log(err.message)
      }

    }

    const handleDelete = (id) => {
      projects.value = projects.value.filter((project) => project.id !== id)
    }

    const handleComplete = (id) => {
      let p = projects.value.find((project) => project.id === id)
      p.complete = !p.complete
    }

    onMounted(() => load())

    return {
      projects,
      handleDelete,
      handleComplete
    }
  },
}
</script>
