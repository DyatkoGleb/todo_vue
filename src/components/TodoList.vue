<template>
    <div class="todo-list">
        <new-todo @createTodo="createTodo"/>
        <transition-group name="todo-list">
            <todo-item
                v-for="todo in newTodoList"
                :key="todo.id"
                :todo="todo"
                @update="update"
                @removeTodo="removeTodo"
                draggable="true"
                class="nope"
            />	
        </transition-group>		
    </div>
</template>

<script>
import NewTodo from '@/components/NewTodo'
import TodoItem from '@/components/TodoItem'

export default {
    // TODO: Drag and drop мб без либ, просто при отпускании узнать текущие координаты и координаты тудушки над этими координатами
    // п.с. заюзать бинарный поиск мб коррдинаты первой и последней / 2, взяnm серединный элемент и узнать выше или ниже середины щас мышка и т.д.  
	components: { TodoItem, NewTodo },
    props: {
        todoList: {
            type: Array,
            required: true
        }
    },
    data() {
        return {
            newTodoList: []
        }
    },
    methods: {
        update(newTodo) {
            this.$emit('update', newTodo)
        },
        createTodo(newTodo) {
            this.$emit('createTodo', newTodo)
        },
        removeTodo(todoId) {
            this.$emit('removeTodo', todoId)
        }
    },
    watch: {
        todoList() {
            this.newTodoList = this.todoList
        }
    }
}
</script>

<style scoped>
.nope {
    background: none;
    border-radius: 12px;
    overflow: hidden;
}
.todo-list {
    width: 100%;
    margin: 60px 5px 0 5px;
    background: transparent;
}
.todo-list-enter-active {
  transition: all 0.2s ease-out;
}

.todo-list-leave-active {
  transition: all 0.2s ease-out;
}

.todo-list-enter-from,
.todo-list-leave-to {
  transform: translateY(20px);
  opacity: 0;
}
</style>