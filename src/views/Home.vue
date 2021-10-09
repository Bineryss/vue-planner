<template>
  <div class="home">
    <FilterNav @filter-change="current = $event" :current="current"/>
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
  </div>
</template>

<script>

import {computed, onMounted, ref} from "vue";
import SingleProject from "@/components/SingleProject";
import FilterNav from "@/components/FilterNav";

export default {
  name: 'Home',
  components: {FilterNav, SingleProject},
  setup() {
    const current = ref('all')
    const projects = ref([])

    let filteredProjects = computed(() => {
      switch (current.value) {
        case 'all':
          return projects.value
        case 'completed':
          return projects.value.filter((project => project.complete))
        case 'ongoing':
          return projects.value.filter((project => !project.complete))
      }
    })

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
      current,
      projects,
      filteredProjects,

      handleDelete,
      handleComplete
    }
  },
}
</script>

