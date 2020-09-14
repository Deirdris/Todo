<template>
  <div class="home">
    <div id="name">
      <h3>My todo list</h3>
    </div>

    <div id="input">
      <input type="text" id="todo" v-model.trim="newTodo">
      <button id="add" v-on:click="add"><i class="far fa-plus-square"></i></button>
    </div>

    <div id="todos" >
      <div v-for="(todo,i) in todosFilter" :key="i" style="text-align: center; padding-bottom: 30px;">

        <span style="padding-right: 20px;">
         <input type="checkbox" id="done" v-model="todo.completed">
        </span>

       <span v-if="!todo.editing" class="todo-item-label" @dblclick="editTodo(todo)" :class="{completed : todo.completed}">
       {{todo.title}}
       </span>

       <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEditing(todo)" @keyup.enter="doneEditing(todo)" @keyup.esc="cancelEditing(todo)">

         <button id="remove" v-on:click="removeTodo(i)"><i class="fas fa-times"></i></button>

      </div>
    </div>

    <div style="display: flex; align-items: center; justify-content: center">

      <div class="extras">

        <label><input type="checkbox" :checked="!allDone" @change="checkAll">Check all</label>
        <span>{{remaining}} items left</span>

      </div>

    </div>

    <div style="display: flex; align-items: center; justify-content: center">

    <div class="extras">

        <button :class="{active: filter === 'all'}" @click="filter = 'all'">All</button>
        <button :class="{active: filter === 'active'}" @click="filter = 'active'">Active</button>
        <button :class="{active: filter === 'completed'}" @click="filter = 'completed'">Completed</button>

    </div>

  </div>

  </div>
</template>

<script>

export default {
  name: 'Home',
  components: {

  },
  data() {
    return {
      newTodo: '',
      beforeEdit: '',
      filter: 'all',
      todos:[

      ]

    }
  },
  methods: {
    add(){
      if(this.newTodo.length === 0) {
        return
      }
        this.todos.push({
          title: this.newTodo,
          completed: false,
          editing: false
        })


      this.newTodo = ''
    },
    removeTodo(i){
      this.todos.splice(i,1)
    },
    editTodo(todo){
      this.beforeEdit = todo.title;
      todo.editing = true;
    },
    doneEditing(todo){
      if(todo.title.length === 0){
        todo.title = this.beforeEdit;
      }
      todo.editing = false;
    },
    cancelEditing(todo){
      todo.title = this.beforeEdit;
      todo.editing = false;
    },
    checkAll(){
      this.todos.forEach(todo => todo.completed = event.target.checked)
    }


  },
  computed: {
    remaining(){
      return this.todos.filter(todo => !todo.completed).length;
    },
    allDone(){
      return this.remaining !== 0;
    },
    todosFilter(){
      if(this.filter === 'all'){
        return this.todos;
      } else if (this.filter === 'active'){
        return this.todos.filter(todo => !todo.completed);
      } else if(this.filter === 'completed'){
        return this.todos.filter(todo => todo.completed);
      }
    }
  }

}
</script>

<style lang="scss">

 .home{

 }

 #name{

   display: flex;
   justify-content: center;
   align-content: center;

   h3{
     font-family: 'Dosis', sans-serif;
     font-size: 60px;
     color: #18189e;
     padding-top: 40px;
   }
 }

 #input{
   display: flex;
   justify-content: center;
   align-content: center;

    input{
      border-style: none;
      padding: 25px;
      font-size: 20px;
      width: 20%;
      font-family: 'Dosis', sans-serif;
      border-radius: 40px;
      color: #18189e;

      &:focus{
        outline: none;
      }
    }

   #add{
     border: none;
     background: none;
     font-size: 50px;
     color: #18189e;
     cursor: pointer;
     padding-left: 25px;
     &:focus{
       outline: none;
     }
   }
 }
 
 #todos{
   font-family: 'Dosis', sans-serif;
   color: #18189e;
   font-size: 30px;
   padding-top: 20px;
   font-weight: bold;
   max-width: 20%;
   margin: 0 auto;
   #remove{
       cursor: pointer;
       font-size: 20px;
       border: none;
       background: none;
       color: #18189e;
       &:focus{
         outline: none;
       }
   }
 }

 .todo-item-edit{
   border-style: none;
   padding: 25px;
   font-size: 20px;
   font-family: 'Dosis', sans-serif;
   border-radius: 40px;
   color: #18189e;
   &:focus{
     outline: none;
   }
 }

 .completed{
   text-decoration: line-through;
   color: dimgray;
 }

 .extras{
   width: 30%;
   display: flex;
   align-items: center;
   justify-content: space-between;
   font-size: 20px;
   border-top: 3px solid darkblue;
   padding-top: 20px;
   margin-bottom: 20px;
   color: #18189e;
   font-family: 'Dosis', sans-serif;

   button{
     font-size: 20px;
     background-color: whitesmoke;
     appearance: none;
     border: none;
     border-radius: 5px;
     padding: 5px 12px 5px 12px;
     font-family: 'Dosis', sans-serif;
     color: black;

     &:hover{
       background: cornflowerblue;
     }
     &:focus{
       outline: none;
     }
   }

   .active{
     background: darken(cornflowerblue,20%);
   }
 }

</style>
