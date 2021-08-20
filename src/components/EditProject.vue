<template>
    <form >
        <label> Title </label>
        <input type="text" v-model="title">
        <label> Details </label>
        <textarea required v-model="details"></textarea>
        <button @click.prevent ='HandleUpdate' > Update project</button>
    </form>
</template>

<script>
export default {
  props: ['id'],

  data(){
    return {
      title: '',
      details: '',
      uri: 'http://localhost:3000/projects/' + this.id
    }
  },
  mounted() {
    fetch(this.uri)
    .then( res=> res.json())
    .then( data => {
      this.title = data.title;
      this.details = data.details;
    })
  },
  methods: {
    HandleUpdate() {
      
      fetch(this.uri, { 
                    method: 'PATCH',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({ title: this.title, details: this.details })                  
                }).then(() => this.$router.push('/'))
                  .catch(e=>console.log(e.message))
            }
    }
  }

</script>

<style>

</style>