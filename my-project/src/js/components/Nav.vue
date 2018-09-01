<template>
<nav class="navbar navbar-expand-lg navbar-dark navbar-custom fixed-top">
  <b-navbar-toggle target="nav_collapse"></b-navbar-toggle>
  <b-navbar-brand href="./">NavBar</b-navbar-brand>

  <b-collapse is-nav id="nav_collapse">

    <!-- Right aligned nav items -->
    <b-navbar-nav v-if="links" class="ml-auto">
     <!-- <b-nav-item href="./About">About</b-nav-item> -->
     <b-nav-item href="./Signup">Sign Up</b-nav-item>
      <b-nav-item href="./Login">Log In</b-nav-item>
    </b-navbar-nav>
    <b-navbar-nav v-if="logoutlink" class="ml-auto">
      
      <b-nav-item href="./Gallery">Gallery</b-nav-item>
      <b-nav-item href="./Users">Users</b-nav-item>
      <b-nav-item href="./Login" @click="logout">Logout</b-nav-item>
    </b-navbar-nav>

  </b-collapse>
</nav>
</template>

<script>
export default {
  name: 'nav',
   data () {
    return { 
      links:false,
      logoutlink:false
      }
  },
  methods: {
    logout() {
      this.$session.destroy()
        location.href="/login"
    }
  },
    created () {
      var self=this
      if(self.$session.exists()){
        this.logoutlink=true
      }
     if(!self.$session.exists()){
        this.links=true        
      }
    }
};
</script>

<style scope>

</style>


