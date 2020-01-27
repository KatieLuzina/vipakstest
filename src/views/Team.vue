<template>
  <div class="myTeam">
    <div  class="column">
    <h2>Моя команда</h2>
    <div class="user_info" v-for="item in myteam" :key="item.login">     
      <img class ="myTeam_img" :src="item.avatar_url">
      <div class = "user_descr">     
        {{item.login}}
        <br>
        <a :href="item.html_url">{{item.html_url}}</a>
      </div>
    </div>
    </div>
    <div  class="column">
    <h2>Пользователи</h2>
    <div class = "user_info" v-for="item in users" :key="item.login">      
      <img class ="myTeam_img" :src="item.avatar_url">
      <div class = "user_descr">
        {{item.login}}
        <br>
        <a :href="item.html_url">{{item.html_url}}</a>   
      </div>  
    </div>
    </div>
  </div>
</template>


<script>
import axios from 'axios'
export default {
  name: 'Team',
  data() {
    return {
      users : [],
      myteam : []
    }
  },
  mounted() {
      axios.get('https://api.github.com/users?since=50000000')
      .then(response => {
        this.users = response.data
        for (let i=0 ;i<5 ;i++ ) {
          this.myteam.push(this.users[i])
          this.users.shift()        
      }

      })


  }
}

</script>

<style>
.myTeam {
  display: flex;
  justify-content: center;
}
.column {
  display: flex;
  flex-direction: column;
  margin: 10px 50px;
}
.user_info {
  display: flex;
  margin: 30px;
}
.user_info img {
  width: 150px;
  
}
.user_descr {
  margin: 30px;
}
</style>
