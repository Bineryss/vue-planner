<template>
  <ProjectForm :title="title" :details="details" submitText="Update Project" @submit-data="updateProject"/>
</template>

<script>
import ProjectForm from "@/components/ProjectForm";
import {onMounted, ref} from "vue";
import {useRouter} from "vue-router";

export default {
  name: "EditProject",
  components: {ProjectForm},
  props: ['id'],
  setup(props) {
    const title = ref('')
    const details = ref('')

    const uri = "http://localhost:3000/projects/" + props.id
    const router = useRouter()

    const loadProject = async () => {
      try {
        const data = await fetch(uri)
        const project = await data.json()
        title.value = project.title
        details.value = project.details
      } catch (err) {
        console.log(err.message)
      }
    }

    const updateProject = async (title, details) => {
      try {
        await fetch(uri, {
          method: 'PATCH',
          headers: {'Content-Type': 'application/json'},
          body: JSON.stringify({
            title: title,
            details: details
          })
        })
        await router.push('/')
      } catch (err) {
        console.log(err.message)
      }
    }

    onMounted(loadProject)
    return {
      title,
      details,
      updateProject
    }
  }
}
</script>

<style scoped>

</style>