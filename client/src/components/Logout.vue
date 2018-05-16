<template>
  <div>
    <button type="button" class="btn btn-danger" @click="logout()">
      Logout
    </button>
  </div>

</template>

<script>
export default {
  name:'logoutVue',
  methods:{
    logout:function(){
      localStorage.removeItem('token')
      window.location.reload()
    }
  }
}
</script>
