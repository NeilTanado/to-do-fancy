<template lang="html">
  <div>
    <!-- Button trigger modal register -->
    <button type="button" class="btn btn-primary mr-3" data-toggle="modal" data-target="#exampleRegister">
      Register
    </button>

    <!-- Modal register -->
    <div class="modal fade" id="exampleRegister" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Register</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div class="limiter">
              <div class="p-t-31 p-b-9">
                <span class="txt1">
                  email
                </span>
              </div>
                <input class="input100" type="email" placeholder="email as username" v-model="email">
              <div class="p-t-13 p-b-9">
                <span class="txt1">
                  Password
                </span>
              </div>
              <div class="wrap-input100 validate-input" data-validate = "Password is required">
                <input class="input100" type="password" name="pass" placeholder="put your password.." v-model="password">
                <span class="focus-input100"></span>
              </div>
              <div class="p-t-31 p-b-9">
                <span class="txt1">
                  name
                </span><br>
              </div>
                <input class="input100" type="text" placeholder="put your name.." v-model="name">
              <div class="p-t-13 p-b-9">
                <button class="btn btn-info mt-3 mb-3" @click="register()">
                  Register
                </button>
                </div>
                <facebook-login class="button"
                  appId="127049751494903"
                  @login="getLoginData"
                  @logout="onLogout"
                  @get-initial-status="getLoginData">
                </facebook-login>
              </div>
              <div class="modal-footer mt-4">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
              </div>
            </div>
          </div>
        </div>
      <!-- end modal register -->
  </div>
</div>
</template>

<script>
import axios from 'axios'
import facebookLogin from 'facebook-login-vuejs'

export default{
  name:'registerVue',
  data(){
    return{
      username:'',
      password:'',
      name:''
    }
  },components:{
    facebookLogin
  },
  methods:{
    register: function(){
      let createUser = {
        email : this.email,
        password : this.password,
        name : this.name
      }
      axios.post('http://localhost:3000/users/createuser',createUser,{})
        .then((value) => {
          console.log('sukses');
          $('#exampleRegister').modal('hide');
        })
        .catch((err) => {
          console.log('err');
        })
    },
    getUserData(){
      FB.getLoginStatus(function(response) {
        FB.api('/me',{fields:['name','email']},(data)=>{
          let createUser = {
            email : data.email,
            name :data.name
          }
          axios.post('http://localhost:3000/users/createfacebook',createUser)
          .then((value) => {
            console.log(value);
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
