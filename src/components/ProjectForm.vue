<template>
  <form @submit.prevent="handleSubmit">
    <label>Titel</label>
    <input type="text" v-model="title" required>
    <label>Details</label>
    <textarea v-model="details"></textarea>
    <button>{{ submitText }}</button>
  </form>
</template>

<script>

export default {
  name: "ProjectForm",
  props: ['title', 'details', 'submitText'],
  setup(props, context) {

    const handleSubmit = () => {
      context.emit('submit-data',
          props.title,
          props.details
      )
    }

    return {
      handleSubmit
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