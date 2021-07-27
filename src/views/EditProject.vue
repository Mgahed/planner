<template>
  <form @submit.prevent="handle_submit">
    <label for="title">Title:</label>
    <input type="text" v-model="title" id="title" required>
    <label for="details">Details:</label>
    <textarea required v-model="details" id="details" cols="30" rows="10"></textarea>
    <button>Update Project</button>
  </form>
</template>

<script>
export default {
  name: "EditProject",
  props: ['id'],
  data() {
    return {
      title: '',
      details: '',
      uri: 'http://localhost:3000/projects' + this.id
    }
  },
  mounted() {
    fetch(this.uri)
        .then(res => res.json())
        .then(data => {
          this.title = data.title
          this.details = data.details
        })
        .catch(err => console.log(err))
  },
  methods: {
    handle_submit() {
      let project = {
        title: this.title,
        details: this.details,
        // complete: false
      }
      fetch(this.uri, {
        method: 'PATCH',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify(project)
      })
          .then(() => {
            this.$router.push({name: 'Home'})
          })
          .catch(err => console.log(err))
    }
  }
}
</script>

<style>
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
  height: 100px;
}

form button {
  cursor: pointer;
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