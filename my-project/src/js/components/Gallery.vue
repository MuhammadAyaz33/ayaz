
<template>
    <div>
        <div class="cover-fix">
        </div> 
        <b-container class="bv-example-row">
            <b-row>
                <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=81" center fluid-grow width="400" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=83" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=84" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=85" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <div class="w-100"></div>
                 <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=90" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=87" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=88" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=89" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <div class="w-100"></div>
                 <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=91" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=92" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=93" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=94" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <div class="w-100"></div>
                 <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=95" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=96" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=98" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>
                <b-col sm="12" md="6" lg="3">
                    <b-img-lazy src="https://picsum.photos/600/400/?image=99" center fluid-grow width="600" height="400" blank-color="#bbb" alt="img" class="my-2" />
                </b-col>

            </b-row>
        </b-container> 
    </div>
</template> 

<script>
import axios from 'axios';
export default {
  name: 'galery',
  beforeCreate() {
        if (!this.$session.exists()) {
        location.href="/login"
    }
},
  data () {
     return {
      sub:{
        email:''
      },
      message:"",
      SubscribeAlert:false,
      SubscribeSuccess:false,
      SubscribeFail:false,
      dismissSecs:3,
      dismissCountDown: 0,
}
  },
  
  methods: {
    Subscribe(){
      this.SubscribeAlert=false
      this.SubscribeSuccess=false
      this.SubscribeFail=false
      this.message=""

      var url='http://localhost:8014/subscriber';
      var data=JSON.stringify(this.sub)
        axios.post(url,data)
        .then(response => {
          console.log(response.data.message)
        if (this.sub.email==""){
          this.SubscribeAlert=true
          this.message=response.data.message
          this.dismissCountDown = this.dismissSecs
        }
        else if (response.data.success){
          this.SubscribeSuccess=true
          this.message=response.data.message
          this.sub.email=""
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
    }
  },
 
  // beforeCreate: function () {
  //   if (!this.$session.exists()) {
  //       location.href="/login"
  //   }
  // }
};

</script>

<style scope>
.cover-fix{
  margin-top: 120px;
}
</style>


