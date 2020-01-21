<template>
  <div>
    {{info.login}}
  </div>
</template>

<script>
import axios from 'axios';


export default {
  name: 'Profile',
  data() {
    return {
      info: []
    }
  },
  created() {
      axios.get('https://api.github.com/users/KatieLuzina')
      .then(response => {
        this.info= response.data
      })   
  }
}

</script>
