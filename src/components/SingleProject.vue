<template>
  <div class="project" :class="{complete: project.complete}">
    <div class="actions">
      <h3 @click="toggle_details">{{ project.title }}</h3>
      <div class="icons">
        <router-link :to="{name: 'EditProject', params: {id: project.id }}">
          <span class="material-icons">edit</span>
        </router-link>
        <span @click="delete_project" class="material-icons">delete</span>
        <span @click="toggle_complete" class="material-icons tick">done</span>
      </div>
    </div>
    <div v-if="details" class="details">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "SingleProject",
  props: ['project'],
  data() {
    return {
      details: false,
      uri: 'http://localhost:3000/projects/' + this.project.id
    }
  },
  methods: {
    toggle_details() {
      this.details = !this.details
    },
    delete_project() {
      fetch(this.uri, {method: 'Delete'})
          .then(() => this.$emit('delete', this.project.id))
          .catch(err => console.log(err))
    },
    toggle_complete() {
      fetch(this.uri, {
        method: 'PATCH',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify({complete: !this.project.complete})
      })
          .then(() => this.$emit('complete', this.project.id))
          .catch(err => console.log(err))
    }
  }
}
</script>

<style>
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

.project.complete .tick {
  color: #00ce89;
}

h3 {
  cursor: pointer;
}

.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.material-icons {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}

.material-icons:hover {
  color: #777;
}
</style>