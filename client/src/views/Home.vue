<template>
  <div class="home">
    <Header/>
    <div v-if="isLogin">
      <Todo/>
    </div>
    <div v-if="!isLogin">

    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue'
import Todo from '@/components/Todo.vue'
export default {
  name: 'home',
  components: {
    Header,
    Todo
  },
  data(){
    return{
      isLogin : false
    }
  },
  created(){
    if(localStorage.getItem('token')){
      this.isLogin = true
    }
  },
  methods:{
    toogleLogin:function(){
      this.isLogin = false
      console.log('masuk login')
    },
    toggleLogout:function(){
      this.isLogin = true
    }
  }
}
</script>
