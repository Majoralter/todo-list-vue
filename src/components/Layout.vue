<template>
    <Nav @clearStorage="removeTodosItem" :isCollapsed="isCollapsed" @open="expandForm" />

    <main>
        <form v-show="!isCollapsed" @submit.prevent="addNewTodo">
            <span class="badge" @click="isCollapsed = !isCollapsed">
                collapse
            </span>

            <label>
                Create a new task
                <input type="text" v-model="todo" placeholder="Task description">
            </label>

            <button type="submit" @click="addNewTodo">
                Add Todo
            </button>
        </form>

        <div class="list">
            <Transition name="fade">
                <p v-if="todos.length === 0">
                    You have <strong>zero</strong> tasks! :D
                </p>
            </Transition>

            <TransitionGroup name="list" tag="ul">
                <li 
                v-if="todos.length > 0"
                class="list--item"
                v-for="(todo, index) in todos" 
                :key="index"
                :class="{completed: todo.isChecked}">
                
                    <span>{{ todo.todoDesc }}</span>

                    <span class="controls">
                        <input type="checkbox" @change="todo.isChecked = !todo.isChecked">
                        <button @click="editTodo(todo.todoDesc, todo)" :disabled="todo.isChecked">Edit</button>
                        <button @click="deleteTodo(todo)">Delete</button>
                    </span>
                </li>
            </TransitionGroup>
        </div>
    </main>
</template>

<script>
import Nav from './Nav.vue';

export default {
    components: {
        Nav
    },

    mounted() {
        if (JSON.parse(localStorage.getItem('todos')) !== null) {
            this.todos = JSON.parse(localStorage.getItem('todos'))
        } else {
            this.todos = []
        }
    },

    data() {
        return {
            isCollapsed: false,
            todos: [],
            todo: ""
        }
    },

    methods: {
        removeTodosItem() {
            this.todos = []
            localStorage.removeItem('todos')
        },

        expandForm() {
            this.isCollapsed = false
        },

        addNewTodo() {
            if (this.todo !== '' && !this.todos.includes(this.todo)) {
                this.todos.push({
                    todoDesc: this.todo,
                    isChecked: false
                })
                this.todo = ''
            }

            localStorage.setItem('todos', JSON.stringify(this.todos))
        },

        deleteTodo(todo) {
            this.todos = this.todos.filter(item => {
                return item !== todo
            })

            localStorage.setItem('todos', JSON.stringify(this.todos))
        },

        editTodo(todoValue, todo) {
            this.todo = todoValue

            console.log(todoValue)

            this.todos = this.todos.filter(item => {
                return item !== todo
            })

            localStorage.setItem('todos', JSON.stringify(this.todos))
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../globals.scss';

main {
    @include flex(column, center, flex-start, var(--size-7));

    p {
        color: #aaa;
    }

    form {
        padding: 15px;
        border: dashed 1px #aaa;
        @include flex(column, center, space-between, 1em);
        width: 30%;
        position: relative;

        .badge{
            position: absolute;
            top: 0;
            right: 0;
            padding: .1em;
            color: #fff;
            background-color: red;
            cursor: pointer;
        }

        label {
            @include flex(column, flex-start, flex-start, .4em);
            width: 100%;
            color: #aaa;

            input {
                width: 100%;
                padding: 12px;
                border: none;
                background-color: #333;
                color: #aaa;
            }
        }

        button {
            all: unset;
            cursor: pointer;
            padding: 12px;
            width: 95%;
            background-color: coral;
            @include flex(row, center, center, 0);
            font-weight: bold;
        }
    }

    .list {
        @include flex(column, center, center, 1rem);

        .list--item {
            background-color: #222;
            @include flex(row, center, space-between, 0);
            width: 40dvw;
            border: solid 1px #aaa;
            padding: 10px;
            margin-block: 10px;

            &.completed{
                border: none;
                background-color: #151515;

                span{
                    color: #555;
                }
            }

            span {
                color: #fff;
                font-size: var(--font-size-2);
            }

            .controls {
                @include flex(row, center, center, 1em);

                button {
                    all: unset;
                    padding: 7px;
                    color: #222;
                    background-color: #aaa;
                    border-radius: 7px;
                    cursor: pointer;
                }

                button:last-of-type {
                    background-color: red;
                    color: #aaa;
                }
            }
        }
    }
}

@media (max-width: 900px) {
    main{
        form{
            width: 85%;
        }

        .list{
            .list--item{
                width: 90dvw;
            }
        }
    }
}
</style>