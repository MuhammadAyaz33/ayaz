
<template>
  <div>
    <div class="set-div"></div>
    <b-table striped hover :items="userList"></b-table>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'users',
  data () {
     return {
      userList:[],
      subTag:false
    }
  },
    beforeCreate() {
            if (!this.$session.exists()) {
            location.href="/login"
        }
    },
    created() {
        this.GetAllUsers();
    },
    


 
  methods: {
        GetAllUsers(){
         
        var url = 'http://localhost:8081/getUsers';
        axios.get(url)
        .then(response => {
            this.userList = response.data
      });
    }
  
  }
}

</script>

<style scope>
.set-div{
    margin-top:80px;
}
</style>


