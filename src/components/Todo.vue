<template>
    <div class="line">
       <span v-if="!editing" class="todo-item-label" @dblclick="edit" :class="{completed : todo.completed}">
           {{ todo.title }}
       </span>
        <input v-else class="todo-item-edit" type="text" ref='input' :value='value' @blur="save"
               @keyup.enter="save" @keydown.esc="editing = false">
        <span class="check" @click='$emit("done")'><i class="fas fa-check"></i></span>
        <span class="trash" @click='$emit("remove")'><i class="fas fa-trash-alt"></i></span>
    </div>
</template>

<script>
    export default {
        name: "Todo",
        props: {
            todo: Object,
            value: String
        },

        data(){
            return {
                editing: false
            }
        },

        methods: {
            edit(){
                this.editing = true;
                this.$nextTick(() => this.$refs.input.focus());
            },

            save(e){
                let { value } = e.target;
                if(value && this.editing){
                    this.$emit('input', value);
                }

                this.editing = false;
            }
        }
    }
</script>

<style lang='scss'>
    .line {
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

    .todo-item-edit {
        border-style: none;
        padding: 8px 10px;
        font-size: 30px;
        font-family: 'Dosis', sans-serif;
        color: dimgrey;
        flex-grow: 1;

        &:focus {
            outline: none;
        }
    }

    .completed {
        text-decoration: line-through;
        color: dimgray;
    }
</style>
