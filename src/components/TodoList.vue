<template>
        <div class="todo-list">
            <new-todo @createTodo="createTodo"/>
            <transition-group name="todo-list">
                <todo-item
                    v-for="todo in todoList"
                    :key="todo.id"
                    :todo="todo"
                    @update="update"
                    @removeTodo="removeTodo"
                />	
            </transition-group>		
        </div>
</template>

<script>
import NewTodo from '@/components/NewTodo'
import TodoItem from '@/components/TodoItem'

export default {
	components: { TodoItem, NewTodo },
    props: {
        todoList: {
            type: Array,
            required: true
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
    }
}
</script>

<style scoped>
.todo-list {
    width: 100%;
    margin: 60px 5px 0 5px;
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