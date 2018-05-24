<template>
  <div>
  
    <h2>Create a Todo List</h2>
  
    <form @submit.prevent>
  
      <div class="form-group">
  
        <input type="text" class="form-control" @keypress="typing=true" placeholder="What do you want to do?" v-model="todo" @keyup.enter="addTodo($event)">
  
        <span class="help-block small text-center" v-show="typing">Hit enter to save</span>
  
      </div>
  
    </form>
  </div>
</template>

<script>
  import axios from 'axios';  
  import bus from "./../bus.js";
  
  export default {
    data() {
      return {
        todo: '',
        typing: false,
      }
  
    },
  
    methods: {
  
      addTodo(event) {
        firebase.firestore().collection("todos").add({
          name: this.todo,
          done: false          
        });
        this.clearTodo();
        this.typing = false;
  
      },
  
      clearTodo() {
  
        this.todo = '';
  
      },
  
      refreshTodo() {
  
        bus.$emit("refreshTodo");
  
      }
  
    }
  
  }
</script>
