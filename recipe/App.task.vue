<template>
  <div id="app">
    <div id="nav">
      <router-link :to="{ name: 'main' }">Home Page </router-link>|
      <router-link :to="{ name: 'search' }">Search </router-link>|
      
        <router-link :to="{ name: 'register' }">Register </router-link>|
        <router-link :to="{ name: 'login' }">Login </router-link>|
         <router-link :to="{ name: 'about' }">About </router-link>
         <div id="log">
           
         <b><u>Hello:</u></b> 
     <span v-if="!$root.store.username">
       Guest 
      </span>
      <span v-else>
        
      <b> {{  $root.store.username }} </b> 
      <div class="dropdown">
        <button class="dropbtn"><U>Personal</U></button>
        <div class="dropdown-content">
          <router-link :to="{ name: 'favorite' }">Favorite </router-link>
          <router-link :to="{ name: 'personal' }">Personal </router-link>
        <a href="#">Family</a>
        </div>
      </div>
       <button @click="Logout">Logout</button>
      

      </span>
         </div>
    </div>
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",
  methods: {
   async Logout() {
       try {
      
        const response = await this.axios.post(
       
          "http://localhost:3000/user/Logout",

        );
       
         } catch (err) {
              
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
       
      
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
       
      });
       
    }
  }
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
}

#nav {
  padding: 26px;
  background-color: aqua;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
  
}

#nav a.router-link-exact-active {
  color: #029955;
}
#log{
  word-spacing: 10px;
  position: absolute;
  left: 900px;
  margin-top: -35px;
}

.dropbtn {
   background-color: aqua;
  color: rgb(14, 12, 12);
  padding: 12px;
  font-size: 15px;
  border: none;
  cursor: pointer;
}

.dropdown {
  position: relative;
  display: inline-block;
}

.dropdown-content {
  display: none;
  position: absolute;
  min-width: 30px;
  box-shadow: 0px 4px 8px 0px rgba(0,0,0,0.2);
  z-index: 1;
}

.dropdown-content a {
  color: black;
  padding: 9px 9px;
  text-decoration: none;
  display: block;
 
}

.dropdown-content a:hover {background-color: #f1f1f1}

.dropdown:hover .dropdown-content {
  display: block;
  background-color: rgb(241, 239, 235);
}

.dropdown:hover .dropbtn {
  font-weight:bold; 
}
</style>
