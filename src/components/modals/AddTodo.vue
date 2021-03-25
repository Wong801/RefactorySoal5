<template>
  <div class="addTodo h-full">
    <h1 class="p-10 text-xl font-bold">New Task</h1>
    <form class="flex flex-col px-10 mr-8" @submit="addTodo">
      <label for="title" class="text-2xl font-bold">Title</label>
      <input type="text" name="title" v-model="title" placeholder="Fill title" class="p-4 mt-4">
      <label for="desc" class="mt-4 text-2xl font-bold">Description</label>
      <textarea name="desc" cols="30" v-model="desc" rows="5" class="p-4 mt-4">Fill description</textarea>
      <label for="createdAt" class="mt-4 text-2xl font-bold">Choose Date</label>
      <datepicker name="createdAt" v-model="createdAt" :lang="lang" type="datetime" :format="format" :placeholder="placeholder" class="mt-4 date"></datepicker>
      <div class="buttons">
        <button @click="$emit('close-add-todo')" class="close"><h2>Cancel</h2></button>
        <button type="submit" class="submit ml-8">Create Task</button>
      </div>
    </form>
  </div>
</template>

<script>
import Datepicker from 'vue2-datepicker'
import 'vue2-datepicker/index.css'
import 'vue2-datepicker/locale/id'
import {v4 as uuidv4} from 'uuid'

export default {
  name: 'AddTodo',
  components: {
    Datepicker
  },
  data() {
    return {
      format: 'DD-MMMM-YYYY',
      placeholder: 'Pilih Tanggal',
      lang: 'id',
      time: null,
      title: '',
      desc: '',
      createdAt: ''
    }
  },
  methods: {
    addTodo(e) {
      e.preventDefault();
      if(!this.createdAt) {
        alert('masukkan tanggal')
        return
      } else if(!this.title) {
        alert('masukkan judul')
      }
      const newTodo = {
        id: uuidv4(),
        title: this.title,
        desc: this.desc,
        createdAt: this.createdAt,
        completed: false
      }
      this.$emit('add-todo', newTodo)
    }
  }
}
</script>

<style scoped>
.addTodo {
  border-radius: 20px;
  width: 50vw;
  margin-top: 20rem;
  background-color: #FBFCFF;
}
form * {
  border-radius: 10px;
}
h1, 
label {
  color: #525C77;
}
.date {
  width: 20vw;
}
.buttons {
  margin: 10rem auto;
  width: 20vw;
}
.close {
  border: 1px solid #0442D0;
  background-color: #fff;
  color: #0442D0;
  width: 4rem;
  border-radius: 10px;
}
.submit {
  background-color: #0442D0;
  color: #fff;
  width: 4rem;
  border-radius: 10px;
}
.submit
</style>