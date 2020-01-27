<template>
  <div class="all_users">
    <div  class="column">
    <h2 class="myteam_header" @click="sort()">Моя команда</h2>
    <div class="user_info" v-for="item in preparedMyTeam" :key="item.login">     
      <img class ="myTeam_img" :src="item.avatar_url">
      <div class = "user_descr">     
        {{item.login}}
        <br>
        <a :href="item.html_url">{{item.html_url}}</a>
        <br>
        <button class="users_btn" @click="del(item.login)">Удалить из команды</button>
      </div>
    </div>
    </div>
    <div  class="column">
    <h2>Пользователи</h2>
    <input class="search_users" placeholder="Найти" v-model="filterModel.login">
    <div class = "user_info" v-for="item in preparedUsers" :key="item.login">      
      <img class ="myTeam_img" :src="item.avatar_url">
      <div class = "user_descr">
        {{item.login}}
        <br>
        <a :href="item.html_url">{{item.html_url}}</a>   
        <br>
        <button class="users_btn" @click="add(item.login)">Добавить в команду</button>
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
      sortColumn: 'login',
      sortDirection: 'asc',
      filterModel: {
        login: null
      },
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
  },
  computed : {
    preparedMyTeam() 
    {
      let list=this.sortList(this.myteam, this.sortColumn, this.sortDirection)
      return list
    },
    preparedUsers() 
    {
      let list=this.filterList(this.users)
      return list
    }
  },
  methods : {
    sort() {
      this.sortDirection = this.sortDirection ==='Asc' ? 'Desc' : 'Asc'
    },
    sortList(list, field, direction) {
      if(direction == "Asc")
        return list.sort((a, b) => (a[field] > b[field]) ? 1 : -1)
      else
        return list.sort((a, b) => (a[field] < b[field]) ? 1 : -1)
    },
    filterList(list) {
      if(this.filterModel.login)
        list = list.filter(val => val.login.indexOf(this.filterModel.login) >= 0)
      
      return list
    },
    del(login) {
      let myteam = this.myteam
      let index = 0
      for (let i=0; i<myteam.length; i++)
        if (myteam[i].login == login) index = i
      this.users.push(myteam[index])
      this.myteam.splice(index , 1)   
    },
    add(login) {
      let users = this.users
      let index = 0
      for (let i=0; i<users.length; i++)
        if (users[i].login == login) index = i
      this.myteam.push(users[index])
      this.users.splice(index , 1)   
    }
  }
}
</script>

<style>
.all_users {
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
  text-align: left;
}

.myteam_header {
  cursor: pointer; 
  margin-bottom: 35px;
}
.users_btn {
  width: 200px;
  height: 25px;
  margin-top: 30px; 
  border: 1px solid #42b983;
  background-color: #42b983;
  color: #fff;
  text-transform: uppercase;
}
.search_users {
  width: 400px;
  border: 1px solid #42b983;
  height: 25px;
  margin-left: 30px;
  margin-top: 15px;
}
@media (max-width: 1200px) {
.column {
  margin: 10px 10px;
}
.user_info {
  margin: 10px;
}
.user_descr {
  margin: 10px;
  font-size: 14px;
}
.user_info img {
  width: 100px;  
}
.users_btn {
  width: 200px;
  height: 20px;
  margin-top: 10px; 
  border: 1px solid #42b983;
  background-color: #42b983;
  color: #fff;
  text-transform: uppercase;
}
.search_users {
  width: 250px;
  border: 1px solid #42b983;
  height: 25px;
  margin-left: 30px;
  margin-top: 15px;
}
}
@media (max-width: 767px) {
.all_users {
  display: block;
}
.user_info img {
  width: 80px;  
}
.users_btn {
  width: 150px;
}
.user_descr {
  font-size: 12px;
}
}
</style>