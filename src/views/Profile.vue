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
    jbkdjl
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
      created_at: null
    }
  },
  created() {
      axios.get('https://api.github.com/users/KatieLuzina')
      .then(response => {
        this.info= response.data        
        this.created_at=(moment(response.data.created_at).locale('ru')).format('LLL');
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
  width: 300px;
  margin-right: 50px;
}
.profile_descr {
  font-size: 22px;
  font-weight: bold;
  line-height: 30px;
}
.profile_login {
  margin-bottom: 45px;
}
</style>