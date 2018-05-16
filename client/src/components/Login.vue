<template>
  <div>
  <!-- Button trigger modal login -->
    <button type="button" class="btn btn-primary mr-3" data-toggle="modal" data-target="#exampleModal">
      Login
    </button>
  <!-- Modal login -->
      <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLabel">Login</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <div class="limiter">
                <span>
                  Sign In With
                </span><br>
                <facebook-login class="button"
                  appId="127049751494903"
                  @login="getLoginData"
                  @logout="onLogout"
                  @get-initial-status="getLoginData">
                </facebook-login>
                <div class="p-t-31 p-b-9">
                  <span class="txt1">
                    Username
                  </span>
                </div>
                  <input class="input100" type="email" v-model="username" >
                <div class="p-t-13 p-b-9">
                  <span class="txt1">
                    Password
                  </span>
                </div>
                <div class="wrap-input100 validate-input" data-validate = "Password is required">
                  <input class="input100" type="password" v-model="password" >
                  <span class="focus-input100"></span>
                </div>
                <div class="container-login100-form-btn m-t-17">
                  <button class="btn btn-info mt-3" @click="login()">
                    Sign In
                  </button>
                </div>

                <div class="w-full text-center p-t-55">
                  <span class="txt2">
                    Not a member?
                  </span>

                  <a href="#" class="txt2 bo1">
                    Sign up now
                  </a>
                </div>
              </div>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  <!-- end modal login -->
<script>
import axios from 'axios'
import facebookLogin from 'facebook-login-vuejs'

export default{
  name:'loginVue',
  data(){
    return{
      username:'',
      password:''
    }
  },
  components:{
    facebookLogin
  },
  methods:{
    login: function(){
      let userLogin = {
        email : this.username,
        password : this.password
      }
      axios.post('http://localhost:3000/users/login',userLogin,{})
      .then((userToken) => {
        localStorage.setItem('token',userToken.data.token)
        window.location.reload()
        $('#exampleModal').modal('hide');

      })
      .catch((err) => {
        console.log(err);
      })
    },
    getLoginData(){
      FB.getLoginStatus(function(response) {
        FB.api('/me',{fields:['name','email']},(data)=>{
          let loginUser = {
            email : data.email,
            name :data.name
          }
          axios.post('http://localhost:3000/users/loginfacebook',loginUser)
          .then((userToken) => {
            localStorage.setItem('token',userToken.data.token)
            window.location.reload()
            $('#exampleModal').modal('hide');
          })
          .catch((err) => {
            console.log(err);
          })
        })
      });
    }
  }
}
</script>
