<template>
    <div id="todoapp" class="todoapp">
        <header class="header">
            <h1>todos</h1>
            <input autofocus="autofocus" autocomplete="off" placeholder="还有什么需要做的？" class="new-todo"
                   v-model="newTodo" @keyup.enter="onNewTodoEnter">
        </header>
        <div id="main" class="main" v-if="todoCount > 0">
            <input id="toggle-all" type="checkbox" class="toggle-all" @change="onToggleAllChange"
                   :checked="currentRemainTodo === 0">
            <label for="toggle-all">标记所有为已完成</label>
            <ul id="todo-list" class="todo-list">
                <li class="todo" v-for="todo in filteredTodos" :key="todo.id"
                    :class="{'completed': todo.completed, 'editing': editTodo === todo.id}">
                    <div class="view">
                        <input type="checkbox" class="toggle" @change="onTodoItemToggleChange(todo.id)"
                               :checked="todo.completed">
                        <label @dblclick="onTodoItemDoubleClick(todo.id)">{{todo.content}}</label>
                        <button class="destroy" @click="onDestroyTodoItemClick(todo.id)"></button>
                    </div>
                    <input type="text" class="edit" :id="'todo-edit-'.concat(todo.id)" v-model="todo.content"
                           @blur="onTodoItemEditBlur(todo.id)" @keyup.enter="onTodoItemEditBlur(todo.id)">
                </li>
            </ul>
        </div>
        <div id="footer" class="footer" v-if="todoCount > 0">
            <span class="todo-count">剩余&nbsp;<strong>{{currentRemainTodo}}</strong>&nbsp;项</span>
            <ul class="filters">
                <li><a id="filter-all" @click="filter = 'all'" href="javascript:void(0)"
                       :class="{'selected': filter === 'all'}">全部 </a></li>
                <li><a id="filter-remain" @click="filter = 'remain'" href="javascript:void(0)"
                       :class="{'selected': filter === 'remain'}">进行中</a></li>
                <li><a id="filter-completed" @click="filter = 'completed'" href="javascript:void(0)"
                       :class="{'selected': filter === 'completed'}">已完成</a>
                </li>
            </ul>
            <button id="clear-completed" class="clear-completed" @click="onClearCompletedBtnClick"
                    v-if="todoCount - currentRemainTodo > 0">清除所有已完成
            </button>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'TodoApp',
        data() {
            return {
                newTodo: '',
                editTodo: null,
                todos: [],
                filter: 'all'
            }
        },
        computed: {
            todoCount() {
                return this.todos.length
            },
            currentRemainTodo() {
                let remainTodo = 0;
                for (let i = 0; i < this.todos.length; i++) {
                    if (!this.todos[i].completed) {
                        remainTodo++;
                    }
                }
                return remainTodo;
            },
            filteredTodos() {
                let filteredTodo = [];
                switch (this.filter) {
                    case "all":
                        filteredTodo = this.todos;
                        break;
                    case "completed":
                        for (let i = 0; i < this.todos.length; i++) {
                            if (this.todos[i].completed) {
                                filteredTodo.push(this.todos[i]);
                            }
                        }
                        break;
                    case "remain":
                        for (let i = 0; i < this.todos.length; i++) {
                            if (!this.todos[i].completed) {
                                filteredTodo.push(this.todos[i]);
                            }
                        }
                        break;
                    default:
                }
                return filteredTodo;
            }
        },
        methods: {
            getCurrentMaxId() {
                let maxId = 0;
                for (let i = 0; i < this.todos.length; i++) {
                    maxId = this.todos[i].id > maxId ? this.todos[i].id : maxId;
                }
                return maxId;
            },
            onNewTodoEnter() {
                if (this.newTodo && this.newTodo.trim() !== '') {
                    this.todos.push({"id": this.getCurrentMaxId() + 1, "content": this.newTodo, "completed": false});
                    this.newTodo = '';
                }
            },
            onTodoItemToggleChange(id) {
                for (let i = 0; i < this.todos.length; i++) {
                    if (this.todos[i].id === id) {
                        this.todos[i].completed = !this.todos[i].completed;
                        break;
                    }
                }
            },
            onToggleAllChange(e) {
                const checked = e.target.checked;
                for (let i = 0; i < this.todos.length; i++) {
                    this.todos[i].completed = checked;
                }
            },
            onTodoItemDoubleClick(id) {
                this.editTodo = id;
                setTimeout(function () {
                    document.getElementById('todo-edit-'.concat(id)).focus();
                }, 100);
            },
            onTodoItemEditBlur(id) {
                const newTodo = window.event.target.value;
                if (newTodo && newTodo.trim() !== '') {
                    this.editTodo = null;
                } else {
                    for (let i = 0; i < this.todos.length; i++) {
                        if (this.todos[i].id === id) {
                            this.todos.splice(i, 1);
                            break;
                        }
                    }
                }
            },
            onDestroyTodoItemClick(id) {
                for (let i = 0; i < this.todos.length; i++) {
                    if (this.todos[i].id === id) {
                        this.todos.splice(i, 1);
                        break;
                    }
                }
            },
            onClearCompletedBtnClick() {
                for (let i = 0; i < this.todos.length; i++) {
                    if (this.todos[i].completed) {
                        this.todos.splice(i, 1);
                        i--;
                    }
                }
            }
        }
    }
</script>
