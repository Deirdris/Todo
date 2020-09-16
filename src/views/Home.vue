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
                <Todo v-for="(todo) in todosFilter" :key="todo.id"
                      v-model='todo.title' :todo='todo'
                      @done='todo.completed = !todo.completed'
                      @remove='remove(todo.id)'>
                </Todo>
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
    import Todo from '@/components/Todo';

    export default {
        name: 'Home',
        components: { Todo },
        data(){
            return {
                activeFilter: 'all',
                beforeEdit: '',
                filters: ['all', 'active', 'completed'],
                newTodo: '',
                id: 0,
                todos: []
            }
        },

        methods: {
            add(){
                if(this.newTodo.length === 0){
                    return
                }

                this.todos.push({
                    title: this.newTodo,
                    completed: false,
                    editing: false,
                    id: this.id++
                })

                this.newTodo = ''
            },

            capitalize(text){
                return text[0].toUpperCase() + text.slice(1);
            },

            checkAll(){
                this.todos.forEach(todo => todo.completed = !todo.completed)
            },
            remove(id){

                this.todos = this.todos.filter(todo => todo.id !== id);
            }
        },

        computed: {
            remaining(){
                return this.todos.filter(todo => !todo.completed).length;
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
