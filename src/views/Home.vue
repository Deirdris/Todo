<template>
    <div class="home">
        <div id="name">
            <h3>My todo list</h3>
        </div>
        <div id='main'>
            <div id="input">
                <input type="text" id="todo" v-model.trim="newTodo">
                <span id="add" v-on:click="add"><i class="fas fa-pencil-alt"></i></span>
            </div>
            <div id="todos">
                <div v-for="(todo,i) in todosFilter" :key="i" class="line">
                   <span v-if="!todo.editing" class="todo-item-label" @dblclick="editTodo(todo)"
                         :class="{completed : todo.completed}">
                   {{ todo.title }}
                   </span>
                    <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEditing(todo)"
                           @keyup.enter="doneEditing(todo)" @keyup.esc="cancelEditing(todo)">
                    <span class="check" v-on:click="todoDone(todo)"><i class="fas fa-check"></i></span>
                    <span class="trash" v-on:click="removeTodo(i)"><i class="fas fa-trash-alt"></i></span>
                </div>
            </div>
            <div class='status'>
                <div class="extras">
                    <label><span class="checked-all" @click="checkAll()">
                        <i class="fas fa-dumpster" style="padding-right: 8px;"></i></span>Check all</label>
                    <!--        <label><input type="checkbox" :checked="!allDone" @change="checkAll">Check all</label>-->
                    <span>{{ remaining }} items left</span>
                </div>
            </div>
            <div style='padding: 0 5px;'>
                <div class="extras">
                    <button v-for='filter in filters' :key='filter' :class='{ active: filter === activeFilter }'
                            @click='activeFilter = filter'>{{ capitalize(filter) }}</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        name: 'Home',
        components: {},
        data(){
            return {
                newTodo: '',
                beforeEdit: '',
                activeFilter: 'all',
                filters: ['all', 'active', 'completed'],
                todos: []

            }
        },

        methods: {
            capitalize(text){
                return text[0].toUpperCase() + text.slice(1);
            },

            add(){
                if(this.newTodo.length === 0){
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
                this.todos.splice(i, 1)
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
                this.todos.forEach(todo => todo.completed = !todo.completed)
            },
            todoDone(todo){
                todo.completed = !todo.completed;
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
                if(this.activeFilter === 'all'){
                    return this.todos;
                }else if(this.activeFilter === 'active'){
                    return this.todos.filter(todo => !todo.completed);
                }else if(this.activeFilter === 'completed'){
                    return this.todos.filter(todo => todo.completed);
                }

                return [];
            }
        }

    }
</script>

<style lang="scss">

    .home {
    }

    #name {

        display: flex;
        justify-content: center;
        align-items: center;

        h3 {
            font-family: 'Dosis', sans-serif;
            font-size: 60px;
            color: #cf5f1f;
            padding-top: 40px;
        }
    }

    #main {
        max-width: 450px;
        margin: 0 auto;
    }

    #input {
        display: flex;

        input {
            border-style: none;
            padding: 25px;
            font-size: 25px;
            font-family: 'Dosis', sans-serif;
            color: black;
            flex-grow: 1;

            &:focus {
                outline: none;
            }
        }

        #add {
            border: none;
            background: none;
            font-size: 50px;
            color: black;
            cursor: pointer;
            padding: 0 10px;
            background: #cf5f1f;
            display: flex;
            align-items: center;
            justify-content: center;

            &:hover {
                background: darken(#cf5f1f, 20%);
            }

            &:focus {
                outline: none;
            }
        }
    }

    #todos {
        font-family: 'Dosis', sans-serif;
        color: black;
        font-size: 30px;
        padding: 16px 0;

        .line {
            //padding-bottom: 30px;
            display: flex;

            &:not(:last-child) {
                margin-bottom: 10px;
            }
        }

        .todo-item-label {
            background: whitesmoke;
            padding: 8px 10px;
            flex-grow: 1;
        }

        .check {
            background: limegreen;
            padding: 8px;
            cursor: pointer;

            &:hover {
                background: darken(limegreen, 20%);
            }
        }

        .trash {
            background: red;
            padding: 8px 10px 8px 10px;
            cursor: pointer;

            &:hover {
                background: darken(red, 20%);
            }
        }
    }

    .todo-item-edit {
        border-style: none;
        padding: 8px;
        font-size: 30px;
        margin-right: 15px;
        font-family: 'Dosis', sans-serif;
        color: dimgrey;

        &:focus {
            outline: none;
        }
    }

    .completed {
        text-decoration: line-through;
        color: dimgray;
    }

    .checked-all {
        cursor: pointer;
        color: black;

        &:hover {
            color: darkred;
        }

    }

    .status {
        border-top: 3px solid #cf5f1f;
        border-bottom: 3px solid #cf5f1f;
        padding: 0 10px;
    }

    .extras {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 20px;
        padding: 20px 0;
        color: black;
        font-family: 'Dosis', sans-serif;

        button {
            font-size: 20px;
            background-color: whitesmoke;
            appearance: none;
            border: none;
            padding: 5px 15px 5px 15px;
            font-family: 'Dosis', sans-serif;
            color: black;

            &:hover {
                background: lighten(limegreen, 20%);
            }

            &:focus {
                outline: none;
            }
        }

        .active {
            background: limegreen;
        }
    }

</style>
