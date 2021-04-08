<template>
  <div id="app">
    <ul>
      <li v-for="event in commits" :key="event.url">
          {{event.commit.message}}
      </li>
    </ul>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return {
      commits:[]
    }
  },
  async created(){
    let data = await fetch('https://api.github.com/repos/davidsilvag/commits-list/commits')
    let unorderedCommits = await data.json()
    this.commits = unorderedCommits.sort((a,b)=> new Date(a.commit.author.date)-new Date(b.commit.author.date))
  }
  
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
