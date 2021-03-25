<template>
  <div class="home">
    <div class="sticky z-50">
      <Header v-bind:users="users"/>
    </div>
    <div class="flex">
      <div class="side">
        <div class="card flex mx-8 my-12 text-xl">
          <img src="../assets/Group.png" class="m-6">
          <div class="greet m-4">
            <h1 class="font-bold">Hallo, {{ users.nickname }}!</h1>
            <h2 class="text-white my-2">You have {{ uncompleted }} tasks left for today, Already completed Task today?</h2>
          </div>
        </div>
        <div class="mx-8">
          <datepicker name="createdAt" v-model="createdAt" :lang="lang" :valueType="format" type="datetime" :format="format" :placeholder="placeholder" class="calendar mt-4 date"></datepicker>
        </div>
      </div>
      <div class="todos mx-8 my-12">
        <h1 class="text-3xl font-bold">Task For Today</h1>
        <div class="flex my-8">
          <button class="btn flex text-white" @click="newTodo"><i class="fa fa-plus mr-2 ml-6 my-2 text-sm"></i><p class="ml-2 mr-6 my-1 font-semibold">Create New</p></button>
          <form class="ml-4">
            <input type="text" placeholder="Cari" class="mx-6 text-center">
            <button type="submit" class="mr-6"><i class="fa fa-search"></i></button>
          </form>
        </div>
        <div v-if="toggleAddTodo" class="fixed overflow-x-hidden overflow-y-auto inset-0 flex justify-center items-center z-40 layer2">
          <AddTodo @add-todo="addTodo" @close-add-todo="newTodo"/>
        </div>
        <div class="todo" v-for="(item, index) in users.todos" :key="index">
          <div v-if="item.createdAt === createdAt">
            <div class="todo-card py-4 px-8">
              <div class="flex">
                <h1 class="text-2xl font-bold">{{ item.title }}</h1>
                <div @click="toggleCompleted(item.id)">
                  <button v-if="item.completed" class="flex ml-8 check text-sm"><i class="fa fa-check mt-2"></i><p class="mt-1">Complete</p></button>
                  <button v-else class="flex ml-8 uncheck"><i class="fa fa-check mt-2 text-sm"></i><p class="mt-2 ml-2 text-xs">Set as Complete</p></button>
                </div>
              </div>
              <p class="pre-formatted my-4">{{ item.desc }}</p>
              <div class="flex justify-between">
                <i></i>
                <div class="buttons text-lg">
                  <button><i class="fa fa-pencil mr-8"></i></button>
                  <button @click="deleteTodo(item.id)"><i class="fa fa-trash"></i></button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-if="toggleAddTodo" class="fixed z-30 inset-0 opacity-25 layer"></div> 
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header'
import AddTodo from '@/components/modals/AddTodo'
import Datepicker from 'vue2-datepicker'
import 'vue2-datepicker/index.css'
import 'vue2-datepicker/locale/id'

export default {
  name: 'Home',
  components: {
    Header,
    AddTodo,
    Datepicker
  },
  data() {
    return {
      users:{
        name: 'Samsul Arifin',
        nickname: 'Sam',
        photo: 'https://s3-alpha-sig.figma.com/img/719c/f3b3/00e33f217283df581281fe3a6dd5750a?Expires=1617580800&Signature=AIzG3A52qsVlA~QA7zdzQ2wNVfJBGcPkaDD0TyT8AuInptoxNZKD60l7GOM9x1~eZfW34qxYHtAMmzddPWRMlB82z3LPgnwFmURPUPC1~tLwXdC6PV1Bo68M8J~idU1YctJIJu9sxcmxiMMMyyd69nuflVsXaQv4KeIWBSrTUvVsVKLml0MOvUuOJCUUj5RtCpyEq5w~04HP6p8bgScM303HEtQkqPus6NjSfwKU~6j~kortkYRsZfnaTDOJ2loa6A2G-dhGeagb0KudwYfTEirdveiJLw8hmBpvjPQvww6mhgdAN-eZmRvtsNBwj27sO8GKaxc3u1alRLj67aAnYw__&Key-Pair-Id=APKAINTVSUGEWH5XD5UA',
        todos: [
          {
            id: 1,
            title: 'Meeting With Client A',
            desc: '1. Loream Ipsum Dolor Amet \n 2.Loream Ipsum Dolor Amet',
            createdAt: '04-08-2021',
            completed: false
          },
          {
            id: 2,
            title: 'Meeting With Client B',
            desc: '1. Loream Ipsum Dolor Amet \n 2.Loream Ipsum Dolor Amet',
            createdAt: '04-08-2021',
            completed: false
          }
        ]
      },
      toggleAddTodo: false,
      format: 'DD-MM-YYYY',
      placeholder: 'Pilih Tanggal',
      lang: 'id',
      createdAt: ''
    }
  },
  computed: {
    uncompleted() {
      let res = []
      let obj ={}
      let todo = this.users.todos
      todo.forEach((completed) => {
        if(!completed.completed) {
          obj.completed = completed.completed
          res.push(obj)
        }
      })
      return res.length
    }
  },
  methods: {
    newTodo() {
      this.toggleAddTodo = !this.toggleAddTodo
    },
    deleteTodo(id) {
      this.users.todos = this.users.todos.filter(todo => todo.id !== id);
    },
    addTodo(newTodo) {
      this.users.todos = [...this.users.todos, newTodo]
      this.toggleAddTodo = !this.toggleAddTodo
    },
    toggleCompleted(id) {
      let todo = this.users.todos 
      todo.forEach((data) => {
        if(data.id === id) {
          data.completed = !data.completed
        }
      })
    }
  }
}
</script>

<style scoped>
.side {
  border-right: 1px solid #EEF3FD;
  height: 88.5vh;
}
.card {
  width: 25rem;
  height: 10rem;
  border-radius: 20px;
}
.card h1 {
  color: #FABE2C;
}
.card img {
  width: 5rem;
  height: 5rem;
}
.card, 
.todos .btn {
  background-color: #0442D0;
}
.calendar {
  width: 20vw;
}
.todos .btn {
  border-radius: 20px;
}
.todos .check {
  background-color: #FEEAEB;
  color: #FB3453;
  border-radius: 5px;
  padding: 0 1rem;
}
.todos .uncheck {
  background-color: #F2F4F8;
  color: #D3D5DB;
  border-radius: 5px;
  padding: 0 1rem;
}
form {
  color: #0442D0;
  border: 1px solid #0442D0;
  border-radius: 20px;
  width: 60vw;
}
form input {
  width: 55vw;
  background-color: #FBFCFF;
}
.todos h1 {
  color: #525C77;
}
.todo-card {
  background-color: #fff;
  margin-top: 2rem;
  border-radius: 20px;
}
.pre-formatted {
  white-space: pre;
}
.buttons i {
  color: #525C77;
}
.layer {
  background-color: #000;
}
.layer2 {
  background-color: transparent;
}
</style>