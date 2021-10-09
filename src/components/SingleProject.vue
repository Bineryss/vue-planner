<template>
  <div class="project" :class="{ complete: project.complete}">
    <div class="actions">
      <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
      <div class="icons">
        <router-link :to="{name: 'EditProject', params: {id: project.id}}">
          <ion-icon name="pencil-outline"></ion-icon>
        </router-link>
        <ion-icon name="trash-outline" @click="deleteProject"></ion-icon>
        <ion-icon name="checkmark-outline" @click="toggleComplete"></ion-icon>
      </div>
    </div>
    <div class="details" v-if="showDetails">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
import {ref} from "vue";

export default {
  name: "SingleProject",
  props: ['project'],
  setup(props, context) {
    const uri = 'http://localhost:3000/projects/' + props.project.id
    const showDetails = ref(false)

    const deleteProject = async () => {
      try {
        await fetch(uri, {
          method: 'DELETE'
        })
        context.emit('delete', props.project.id)
      } catch (err) {
        console.log(err.message)
      }
    }
    const toggleComplete = async () => {
      try {
        await fetch(uri, {
          method: 'PATCH',
          headers: {'Content-Type': 'application/json'},
          body: JSON.stringify({complete: !props.project.complete})
        })
      } catch (err) {
        console.log(err.message)
      }
      context.emit('complete', props.project.id)
    }
    return {
      showDetails,
      deleteProject,
      toggleComplete
    }
  }
}
</script>

<style scoped>
.project {
  margin: 20px auto;
  background: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
  border-left: 4px solid #e90074;
}

.project.complete {
  border-left: 4px solid #00ce89;
}

h3 {
  cursor: pointer;
}

.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

ion-icon {
  font-size: 28px;
  margin-left: 15px;
  color: #bbb;
  cursor: pointer;
}

ion-icon:hover {
  color: #777;
}
</style>