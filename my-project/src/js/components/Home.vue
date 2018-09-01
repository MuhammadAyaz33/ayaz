
<template>
  <div>
   <header class="masthead text-center text-white">
      <div class="masthead-content">
        <div class="text-center sub-div" v-if="subTag">
            <b-button variant="primary">
              Subscribers <b-badge variant="light">{{this.subs}}</b-badge>
            </b-button>
          </div>
        <div class="container">
          <div class="flex-container">
            <b-alert v-if="SubscribeAlert" :show="dismissCountDown"
                dismissible 
                variant="danger" 
                @dismissed="dismissCountDown=0"
                @dismiss-count-down="countDownChanged">{{this.message}}
            </b-alert>
            <b-alert v-else-if="SubscribeSuccess" :show="dismissCountDown"
                dismissible 
                variant="success" 
                @dismissed="dismissCountDown=0"
                @dismiss-count-down="countDownChanged">{{this.message}}
            </b-alert>
            <b-alert v-else-if="SubscribeFail" :show="dismissCountDown"
                dismissible 
                variant="danger" 
                @dismissed="dismissCountDown=0"
                @dismiss-count-down="countDownChanged">{{this.message}}
            </b-alert>
          </div>
         
          <h1 class="masthead-heading">Some Text Here</h1>
          <h2 class="masthead-subheading mb-5">Some Text Here Also</h2>
          
          <div class="">
            <input class="main-input" type="text" v-model="sub.email" placeholder="Email"/>
            <button class="btn btn-primary btn-md" @click="Subscribe">Subscribe!</button>
          </div>

        </div>
      </div>
      <div class="bg-circle-1 bg-circle"></div>
      <div class="bg-circle-2 bg-circle"></div>
      <div class="bg-circle-3 bg-circle"></div>
      <div class="bg-circle-4 bg-circle"></div>
    </header>

    <section>
      <div class="container">
        <div class="row align-items-center">
          <div class="col-lg-6 order-lg-2">
            <div class="p-5">
              <img class="img-fluid rounded-circle" src="../../assets/01.jpg" alt="">
            </div>
          </div>
          <div class="col-lg-6 order-lg-1">
            <div class="p-5">
              <h2 class="display-4">For those about to rock...</h2>
              <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quod aliquid, mollitia odio veniam sit iste esse assumenda amet aperiam exercitationem, ea animi blanditiis recusandae! Ratione voluptatum molestiae adipisci, beatae obcaecati.</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section>
      <div class="container">
        <div class="row align-items-center">
          <div class="col-lg-6">
            <div class="p-5">
              <img class="img-fluid rounded-circle" src="../../assets/02.jpg" alt="">
            </div>
          </div>
          <div class="col-lg-6">
            <div class="p-5">
              <h2 class="display-4">We salute you!</h2>
              <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quod aliquid, mollitia odio veniam sit iste esse assumenda amet aperiam exercitationem, ea animi blanditiis recusandae! Ratione voluptatum molestiae adipisci, beatae obcaecati.</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section>
      <div class="container">
        <div class="row align-items-center">
          <div class="col-lg-6 order-lg-2">
            <div class="p-5">
              <img class="img-fluid rounded-circle" src="../../assets/03.jpg" alt="">
            </div>
          </div>
          <div class="col-lg-6 order-lg-1">
            <div class="p-5">
              <h2 class="display-4">Let there be rock!</h2>
              <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quod aliquid, mollitia odio veniam sit iste esse assumenda amet aperiam exercitationem, ea animi blanditiis recusandae! Ratione voluptatum molestiae adipisci, beatae obcaecati.</p>
            </div>
          </div>
        </div>
      </div>
    </section>
    
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'home',
  data () {
     return {
      sub:{
        email:''
      },
      subs:0,
      message:"",
      SubscribeAlert:false,
      SubscribeSuccess:false,
      SubscribeFail:false,
      dismissSecs:3,
      dismissCountDown: 0,
      subTag:false
}
  },
  beforeCreate() {
  },
  created() {
    this.fetchSubscriber();
     if (this.$session.exists()) {
            this.subTag=true
      }
  },
  methods: {
    Subscribe(){
      this.SubscribeAlert=false
      this.SubscribeSuccess=false
      this.SubscribeFail=false
      this.message=""

      var url='http://localhost:8081/subscriber';
      var data=JSON.stringify(this.sub)
        axios.post(url,data)
        .then(response => {
        if (this.sub.email==""){
          this.SubscribeAlert=true
          this.message=response.data.message
          this.dismissCountDown = this.dismissSecs
        }
        else if (response.data.success){
          this.SubscribeSuccess=true
          this.message=response.data.message
          this.sub.email=""
          setTimeout(() => {
           this.fetchSubscriber();
          }, 400);
          this.dismissCountDown = this.dismissSecs
        }
        else if(!response.data.success){
          this.SubscribeFail=true
          this.message=response.data.message
          this.dismissCountDown = this.dismissSecs
          
        }
      });
    },
     countDownChanged (dismissCountDown) {
      this.dismissCountDown = dismissCountDown
     },
  
    fetchSubscriber(){
        this.subs=0
         
        var url = 'http://localhost:8081/getSubscriber';
        axios.get(url,"")
        .then(response => {
          this.subs=response.data
        }),function (err) {
          console.log('err', err)
        }
    }
  }
}
 

</script>

<style scope>
.sub-div{
  position: fixed;
  float: left;
  top: 80px;
}
</style>


