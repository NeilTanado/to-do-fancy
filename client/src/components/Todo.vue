<template>
  <div>
    <div class = "head mt-2 mb-2 mr-2 ml-2">
    <h1>{{quotes}}</h1>
     <input class="inputtext mt-2 mb-2 mr-2 ml-2"
       placeholder="What needs to be done? after you put your activities press enter"
       v-model="todotext"
       @keyup.enter="addTodo">
    <div class="row">
      <div class ="col-md-4"
        <div v-for = "todo in listTodo">
        <div class="card bg-light mb-3 mt-3 ml-3 mr-3" style="max-width: 18rem;">
          <div class="card-header">Todays to do
            <button type="button" class="close" @click="deleteTodo(todo._id)">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="card-body">
            <p class="card-text" v-if="!todo.status">{{todo.description}}</p>
            <p class="card-text" v-else="todo.status"><strike>{{todo.description}}</strike></p>
          </div>
          <div class="card-footer">
            <button class="btn btn-success" v-if="!todo.status" @click="editTodo(todo._id)"><i class="fas fa-check-circle">Done This Task?</i></button>
            <button class="btn btn-danger" v-else="todo.status" @click="editedTodo(todo._id)"><i class="fas fa-exclamation-circle"></i></i>Undone This Task?</i></button>
          </div>
        </div>
      </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Todo',
  data () {
    return {
      quotes : '',
      todotext : '',
      editedText : '',
      listTodo : [],
    }
  },
  methods: {
    editTodo(id){
      let statusTodo = {status : true}
      let tokenUser = localStorage.getItem('token')
      axios.put('http://localhost:3000/todos/update/'+id,statusTodo,{
        headers:{
          tokenUser
        },
      })
      .then((value) => {
        window.location.reload()
      })
      .catch((err) => {
        console.log(err);
      })
    },
    editedTodo(id){
      let statusTodo = {status : false}
      let tokenUser = localStorage.getItem('token')
      axios.put('http://localhost:3000/todos/update/'+id,statusTodo,{
        headers:{
          tokenUser
        },
      })
      .then((value) => {
        window.location.reload()
      })
      .catch((err) => {
        console.log(err);
      })
    },
    deleteTodo (id) {
      let tokenUser = localStorage.getItem('token')
        axios.delete('http://localhost:3000/todos/deletetodo/'+id,{
          headers:{
            tokenUser
          }
        })
        .then((value) => {
          window.location.reload()
        })
        .catch((err) => {
          console.log(err);
        })
    },
    addTodo() {
      this.listTodo.push({description:this.todotext})
      let sentTodo = {
        description : this.todotext
      }
      let tokenUser = localStorage.getItem('token')
      axios.post('http://localhost:3000/todos/create',sentTodo,{
        headers:{
          tokenUser
        }
      })
      .then((response) => {
        window.location.reload()
        swal({
          title: "Grats",
          text: "success add todo!!!",
          icon: "success",
        });
      })
      .catch((err) => {
        console.log(err);
      })
    },
    firstLogin(){
      let tokenUser = localStorage.getItem('token')
      let self = this
      axios.get('http://localhost:3000/todos/datatodo',{
        headers:{
          tokenUser
        }
      })
      .then((value) => {
        self.listTodo = value.data.data.todo
      })
      .catch((err) => {
        console.log(err);
      })
    },
    quoteLogin(){
      axios.get('https://talaikis.com/api/quotes/random/')
      .then((value) => {
        console.log(value.data.quote)
        this.quotes = value.data.quote
      })
      .catch((err) => {
        console.log(err);
      })
    }
  },
  created(){
    this.firstLogin(),
    this.quoteLogin()
  }
}
</script>

<style lang="css">
.inputtext{
  width: 60%
}
h1{
  color : white
}
</style>
