<template>
  <form @submit.prevent="handleSubmit">
    <label>Titel</label>
    <input type="text" v-model="title" required>
    <label>Details</label>
    <textarea v-model="details"></textarea>
    <button>Add Project</button>
  </form>
</template>

<script>
import {ref} from "vue";
import {useRouter} from "vue-router";

export default {
  name: "AddProject",
  setup() {
    const title = ref('')
    const details = ref('')

    const router = useRouter()

    const handleSubmit = async () => {
      const project = {
        title: title.value,
        details: details.value,
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
      title, details, handleSubmit
    }
  }
}
</script>

<style scoped>
form {
  background: white;
  padding: 20px;
  border-radius: 10px;
}

label {
  display: block;
  color: #bbb;
  text-transform: uppercase;
  font-size: 14px;
  font-weight: bold;
  letter-spacing: 1px;
  margin: 20px 0 10px 0;
}

input {
  padding: 10px;
  border: 0;
  border-bottom: 1px solid #ddd;
  width: 100%;
  box-sizing: border-box;
}

textarea {
  border: 1px solid #ddd;
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  height: 100%;
}

form button {
  display: block;
  margin: 20px auto 0;
  background: #00ce89;
  color: white;
  padding: 10px;
  border: 0;
  border-radius: 6px;
  font-size: 16px;
}
</style>