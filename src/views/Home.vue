<template>
  <div class="home">
    <FilterNav @filter-change="current = $event" :current="current"/>
    <div v-if="projects.length">
      <div v-for="project in filtered_projects" :key="project.id">
        <SingleProject :project="project" @delete="handle_delete" @complete="handle_complete"/>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

import SingleProject from "../components/SingleProject";
import FilterNav from "../components/FilterNav";

export default {
  name: 'Home',
  components: {FilterNav, SingleProject},
  data() {
    return {
      projects: [],
      current: 'all'
    }
  },
  mounted() {
    fetch('http://localhost:3000/projects')
        .then(res => res.json())
        .then(data => this.projects = data)
        .catch(err => console.log(err.message))
  },
  methods: {
    handle_delete(id) {
      this.projects = this.projects.filter((project) => {
        return project.id !== id
      })
    },
    handle_complete(id) {
      let project = this.projects.find(project => {
        return project.id === id
      })
      project.complete = !project.complete
    }
  },
  computed: {
    filtered_projects() {
      if (this.current === 'completed') {
        return this.projects.filter((project) => {
          return project.complete
        })
      } else if (this.current === 'on') {
        return this.projects.filter((project) => {
          return !project.complete
        })
      }
      else{
        return this.projects
      }
    }
  }
}
</script>
