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
        <td class="repos_name">{{item.name}}</td>
        <td>
          <b class="repos_name_table">{{item.name}}</b> <br>
          <b>Описание :</b> {{item.description}}<br>
          <b>Язык программирования :</b> {{item.language}}<br>
          <b>Дата создания :</b> {{item.created_at}}<br>
          <b>Ссылка на клонирование репозитория:</b> <a :href="item.clone_url">{{item.clone_url}}</a><br>
        </td>
      </tr>
    </table>
  </div>
  <div class = "profile_following">
    Список подписок 
      <div v-for="item in following.info" :key="item.login">
        <div class = "profile_following_main">
          <img class ="profile_following_img" :src="item.avatar_url">
          <div class = "profile_following_descr">
            Логин : <a class="profile_login" :href="item.html_url">{{item.login}}</a>
          </div>
        </div>        
      </div>
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
          created_at : null,
          languages : []
        } 
      },
      following : {
        info: []
      }
    }
  },
  mounted() {
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
          this.repos.data[i].created_at=(moment(response.data[i].created_at).locale('ru')).format('LLL')     
        }
      })
      axios.get('https://api.github.com/users/KatieLuzina/following')
      .then(response => {
        this.following.info= response.data        
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
table {
  border: 1px solid #ccc;
  border-spacing: 3px;
  border-collapse: collapse;
  margin: 0 auto;
  margin-top: 20px; 
}
td{
  border: solid 1px #ccc;
  text-align: left;
  padding: 5px;
  font-size: 16px;
  font-weight: normal;
}
a{
  color: #42b983;
}
.profile_following {
  margin : 20px 50px;
  font-size: 22px;
  font-weight: bold;
}
.profile_following_main {
  padding: 25px 250px;
  display: flex; 
}
.profile_following_img {
  width: 150px;
  margin-right: 50px;
}
.profile_following_descr {
  font-size: 16px;
  font-weight: bold;
  line-height: 30px;
  padding-top: 20px;
}
.profile_following_login {
  display: block;
  margin-bottom: 40px;
  color: #42b983;
}
.repos_name_table {
  display: none;
  font-weight: bold;
  text-align: center;
}
@media (max-width: 1200px) {
  .profile_main {
    padding: 25px 100px;
  }
  .profile_img {
    width: 250px;
  }
  .profile_following_main {
    padding: 25px 100px; 
  }
}
@media (max-width: 767px) {
  .profile_main {
    padding: 25px 20px;
  }
  .profile_descr {
  font-size: 16px;
  line-height: 16px;
  }
  .profile_img {
    width: 150px;
    margin-right: 20px;
  }
  .profile_login {
  margin-bottom: 0;
  }
  .profile_following_main {
    padding: 25px 20px; 
  }
  td{
  font-size: 14px;
  }
  .profile_following_img {
  width: 100px;
 }
}
@media (max-width: 479px) {
  .profile_main {
  padding: 25px 20px;
  }
  .profile_following_main {
  padding: 25px 20px; 
  }
  td{
  font-size: 12px;
  }
  .profile_following_img {
  width: 60px;
 }
 .repos_name {
   display: none;
 }
 .repos_name_table {
   display: block;
 }
 .profile_repos {
  font-size: 16px;
}
.profile_following {
  font-size: 16px; 
  margin : 10px 20px;
}
.profile_following_descr {
  font-size: 12px;
  line-height: 12px;
  padding-top: 10px;
}
}
</style>