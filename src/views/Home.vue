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
      <div v-for="(todo,i) in todos" :key="i" style="text-align: center">

       <span v-if="!todo.editing" class="todo-item-label" @dblclick="editTodo(todo)">
       {{todo.title}}
       </span>

       <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEditing(todo)" @keyup.enter="doneEditing(todo)">

         <button id="remove" v-on:click="removeTodo(i)"><i class="fas fa-times"></i></button>

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
      todo.editing = true;
    },
    doneEditing(todo){
      todo.editing = false;
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

</style>
