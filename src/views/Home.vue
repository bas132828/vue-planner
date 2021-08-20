<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="projects.length">
       <div v-for="project in filterdProjects" :key="project.id" >
         <SingleProject :project="project" @delete='handleDelete' @complete='handleComplete'/>
       </div>       
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue'

export default {
  name: 'Home',
  components: {
    SingleProject, 
    FilterNav
  },
  data() {
    return {
      projects: [],
      current: 'all'
    }
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then( res => res.json())
      .then( data => {
        this.projects = data;
      })
      .catch( er => console.log( er.message ))
  },
  methods: {
    handleDelete(id) {
     this.projects = this.projects.filter(el=> el.id !== id)
    },
    handleComplete(id) {
      let p = this.projects.find(project => project.id === id)
      p.complete = !p.complete;
    }
  },
  computed: {
    filterdProjects() {
      return this.projects.filter( el=> {
        if (this.current === 'all') return el;
        if (this.current === 'completed') return el.complete === false;
        if (this.current === 'ongoing') return el.complete === true;
      })
    }
  }
}
</script>
