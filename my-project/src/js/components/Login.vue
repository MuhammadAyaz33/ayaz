<template>
  <div class="login-cover">
    <div class="cover-fix"></div>
    <div class="login-form">
      <div class="main-div">
        <b-alert v-if="loginAlert" :show="dismissCountDown"
            dismissible 
            variant="danger" 
            @dismissed="dismissCountDown=0"
            @dismiss-count-down="countDownChanged">{{this.message}}
        </b-alert>
         <b-alert v-else-if="loginFail" :show="dismissCountDown"
            dismissible 
            variant="danger" 
            @dismissed="dismissCountDown=0"
            @dismiss-count-down="countDownChanged">{{this.message}}
        </b-alert>

        <div class="panel">
          <h2>User Login</h2>
          <p>Please enter your email and password</p>
        </div>
        <form v-on:submit.prevent>
          <div class="form-group">
            <input type="email" class="form-control" placeholder="Email" v-model="user.email">
          </div>
          <div class="form-group">
            <input type="password" class="form-control" placeholder="Password" v-model="user.password">
          </div>
          <div class="forgot">
            <a href="reset.html">Forgot password?</a>
          </div>
          <button type="submit" class="btn btn-primary" @click="Login">Login</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';

export default {
  name:'login',
  data () {
    return {
      user: {
            email:'',
            password:''
          },
          message:"",
          loginAlert:false,
          loginFail:false,
          dismissSecs: 3,
          dismissCountDown: 0,
      } 
  },methods: {
       Login() {
        this.loginAlert=false
        this.loginFail=true
        this.message=""

        var url='http://localhost:8081/login';
        var data=JSON.stringify(this.user)
        var info
         axios.post(url,data)
         .then(response => {
          if (this.user.email==""||this.user.password==""){
            this.loginAlert=true
            this.message=response.data.message
            this.dismissCountDown = this.dismissSecs
          }
          else if (response.data.success){
            this.user.email=""
            this.user.password=""
            this.$session.start()
            this.$session.set('token', response.data.token)
            axios.defaults.headers.common['Authorization'] = response.data.token
            location.href="/home"
          }
          else if(!response.data.success){
            this.loginFail=true
            this.message=response.data.message
            this.dismissCountDown = this.dismissSecs
          }
        });
      },
      countDownChanged (dismissCountDown) {
      this.dismissCountDown = dismissCountDown
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cover-fix{
  margin-top: 120px;
}
.login-cover{
   background-image:url("../../assets/login-cover.jpg"); 
   background-repeat:no-repeat; 
   background-position:center; 
   background-size:cover; padding:5px;
   width: 100%;
   height: 600px;
   }
.panel h2{ 
  color:#fff;  
  font-size: 1.3rem; 
  margin:0 0 8px 0;
}
.panel p { 
  color:#fff;  
  font-size: 1.1rem;
  margin-bottom:30px; 
  line-height:24px;
}
.login-form .form-control {
  background:azure none repeat scroll 0 0;
  border: 1px solid gray    ;
  border-radius: 4px;
  font-size: 14px;
  height: 50px;
  margin-bottom: 5px; 
  line-height: 50px;
}
.main-div {
  background: #134E5E;  /* fallback for old browsers */
	background: -webkit-linear-gradient(to left, #71B280, #134E5E);  /* Chrome 10-25, Safari 5.1-6 */
	background: linear-gradient(to left, #71B280, #134E5E); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */

  border-radius: 2px;
  margin: 10px auto 30px;
  max-width: 38%;
  opacity: 0.9;
  padding: 50px 70px 70px 71px;
}


.login-form{ 
  text-align:center;
  margin-top: 50px;
  }
.forgot a {
  color: black;
  font-size: 14px;
  text-decoration: underline;
}
.login-form  .btn.btn-primary {
  background:#134E5E none repeat scroll 0 0;
  border-color:#134E5E;
  color: #ffffff;
  font-size: 1.3rem;
  width: 100%;
  height: 50px;
  line-height: 50px;
  padding: 0;
}
.forgot {
  text-align: left; margin-bottom:30px;
}

.first{
    padding: 5px;
}
.form-control::placeholder{
    color: black;
}
</style>
