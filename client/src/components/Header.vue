<template>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <a class="navbar-brand" href="#">KEGIATANMU.COM</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">
            <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
          </li>
          <li class="nav-item">
            <a class="nav-link disabled" href="#">Put Your Activity Here!</a>
          </li>
        </ul>
        <div v-if="status">
          <div class="navbar-text">
            <Login v-on:toggleLogin="toggleLogin" />
          </div>
          <div class="navbar-text">
            <Register/>
          </div>
        </div>
        <div v-else="!status">
          <Logout v-on:toggleLogout="toggleLogout"></Logout>
        </div>
          </div>
        </div>
      </div>
    </nav>
</template>

<script>
  import Login from '@/components/Login.vue'
  import Register from '@/components/Register.vue'
  import Logout from '@/components/Logout.vue'
  export default{
    name: 'test',
    components:{
      Login,
      Register,
      Logout
    },
    data () {
      return {
        status : true
      }
    },
    created(){
      if(localStorage.getItem('token')){
        this.status = false
      }
    },
    methods: {
      toggleLogin () {
        this.status = !this.status;
      },
      toggleLogout () {
        this.status = true;
      }
    }
  }
</script>
