<template>
  <div id="app">
    <div class="row home">
            <div class="col-12 col-md-6 text-center commit-box flex">
                <div>
                    <h3>Commits History</h3>
                    <ul class="timeline">
                        <li v-for="event in commits" :key="event.sha">
                            <a :href="'https://github.com/DavidSilvaG/commits-list/commit/'+event.sha" target="_blank" data-bs-toggle="tooltip" title="See changes on GitHub">
                                <h5 class="text-info">{{event.commit.message}}</h5>
                            </a>
                            <p>{{formatDate(event.commit.author.date)}} </p>
                        </li>
                    </ul> 
                </div>
            </div>
            <div class="col-12 col-md-6 text-center bg-dark text-white flex">
                <div>
                    <h4>Made with 
                        <a href="https://vuejs.org/" target="_blank">
                            Vuejs
                        </a>, 
                        <a href="https://getbootstrap.com/" target="_blank">
                            Bootstrap
                        </a> and 
                        <a href="https://docs.github.com/en/rest" target="_blank">
                            GitHubApi
                        </a></h4>    
                        
                    <div class="mt-5">
                        <h5>To see commits with GitHubApi make a GET request to:</h5>
                        <h6>
                            https://api.github.com/repos/ {username} / {repoName} /commits/ {sha}
                        </h6>
                        <small>*sha optional to see specific commit</small>
                    </div>
                    <div class="mt-5">
                        Code available on
                        <a class="text-info" href="https://github.com/DavidSilvaG/commits-list" target="_blank">
                            github.com/DavidSilvaG/commits-list
                        </a> 
                    </div>
                </div>
            </div>
        </div>
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
  methods:{
    formatDate(date){
            const months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec']
            let toFormat = new Date(date)
            return `${months[toFormat.getMonth()]} ${toFormat.getDate()} - ${toFormat.getHours()}:${String(toFormat.getMinutes()+100).substring(1,3)}`
    },
  },
  async created(){
    let data = await fetch('https://api.github.com/repos/davidsilvag/commits-list/commits')
    let unorderedCommits = await data.json()
    this.commits = unorderedCommits.sort((a,b)=> new Date(a.commit.author.date)-new Date(b.commit.author.date))
  }
  
}
</script>

<style>
body{
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.home{
    margin: 0;
    min-height: 100vh;
}

.commit-box{
    border: solid 5px #2C3E50;
    box-sizing: border-box;
}

.flex{
    display: flex;
    align-items: center;
    justify-content: center;
}

/* Thanks to https://bootsnipp.com/mylastof*/ 
ul.timeline {
    list-style-type: none;
    position: relative;
}
ul.timeline:before {
    content: ' ';
    background: #d4d9df;
    display: inline-block;
    position: absolute;
    left: 29px;
    width: 2px;
    height: 100%;
    z-index: 400;
}
ul.timeline > li {
    margin: 20px 0;
    padding-left: 20px;
}
ul.timeline > li:before {
    content: ' ';
    background: white;
    display: inline-block;
    position: absolute;
    border-radius: 50%;
    border: 3px solid #22c0e8;
    left: 20px;
    width: 20px;
    height: 20px;
    z-index: 400;
}
</style>
