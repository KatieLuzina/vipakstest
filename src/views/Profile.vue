<template>
<div >
  <div class = "profile_main">
    <img class ="profile_img" :src="info.avatar_url">
    <div class = "profile_descr">
      Логин : <a class="profile_login" :href="info.html_url">{{info.login}}</a>
      <br>
      Дата создания профиля : <div class="profile_createdAt">{{created_at}}</div>
    </div>
  </div>
  <div class = "profile_repos">
    Список репозиториев
    <table>
      <tr v-for="item in repos.data" :key="item.name">
        <td>{{item.name}}</td>
        <td>
          Описание : {{item.description}}
          Язык программирования : 
          <div v-for="(value, name) in item.languages" :key="name">
            {{name}}
          </div>
          Дата создания : {{item.created_at}}
          Ссылка на клонирование репозитория: {{item.clone_url}}
        </td>
      </tr>
    </table>
  </div>
</div>

</template>

<script>
import axios from 'axios';
import moment from 'moment';
export default {
  name: 'Profile',
  data() {
    return {
      info: [],
      created_at: null,
      repos :  {
        data : {
          languages : []
        } 
      }
    }
  },
  created() {
      axios.get('https://api.github.com/users/KatieLuzina')
      .then(response => {
        this.info= response.data        
        this.created_at=(moment(response.data.created_at).locale('ru')).format('LLL');
      })
      axios.get('https://api.github.com/users/KatieLuzina/repos')
      .then(response => {
        this.repos.data= response.data
        for (let i=0; i<this.repos.data.length; i++) {          
          axios.get('https://api.github.com/repos/KatieLuzina/'+ this.repos.data[i].name +'/languages')
          .then(response => {
            this.repos.data[i].languages=response.data
          })          
        }
      })
  }
}
</script>

<style>
.profile_main {
  padding: 25px 250px;
  display: flex; 
}
.profile_img {
  width: 250px;
  margin-right: 50px;
}
.profile_descr {
  font-size: 22px;
  font-weight: bold;
  line-height: 30px;
  padding-top: 20px;
}
.profile_login {
  display: block;
  margin-bottom: 40px;
  color: #42b983;
}
.profile_createdAt {
  color: #42b983;
}
.profile_repos {
  font-size: 22px;
  font-weight: bold;
}
</style>
