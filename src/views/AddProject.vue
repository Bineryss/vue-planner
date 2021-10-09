<template>
  <ProjectForm submit-text="Add Project" @submit-data="handleSubmit"/>
</template>

<script>
import {useRouter} from "vue-router";
import ProjectForm from "@/components/ProjectForm";

export default {
  name: "AddProject",
  components: {ProjectForm},
  setup() {
    const router = useRouter()

    const handleSubmit = async (title, details) => {
      const project = {
        title: title,
        details: details,
        complete: false
      }
      try {

        await fetch('http://localhost:3000/projects', {
          method: 'POST',
          headers: {'Content-Type': 'application/json'},
          body: JSON.stringify(project)
        })
        await router.push('/')
      } catch (err) {
        console.log(err.message)
      }

    }

    return {
      handleSubmit
    }
  }
}
</script>

<style scoped>

</style>