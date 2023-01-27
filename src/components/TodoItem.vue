<template>
    <div 
        class="todo-item"
        :class="{'todo-item--completed': todo.done}"
        @click="updateTask"
        ref="todo"
    >
        <div>
            <div class="todo-item__title" contenteditable @click.stop>
                {{ todo.title }}
            </div>
            <div class="todo-item__description">
                {{ todo.description }}
            </div>
        </div>
        
        <div>
            <a class="todo-item__btn-remove" @click.stop="removeTodo">
                <svg width="12px"  height="12px"  viewBox="0 0 348.333 348.334"  style="enable-background:new 0 0 348.333 348.334;">
                    <g><path d="M336.559,68.611L231.016,174.165l105.543,105.549c15.699,15.705,15.699,41.145,0,56.85   c-7.844,7.844-18.128,11.769-28.407,11.769c-10.296,0-20.581-3.919-28.419-11.769L174.167,231.003L68.609,336.563   c-7.843,7.844-18.128,11.769-28.416,11.769c-10.285,0-20.563-3.919-28.413-11.769c-15.699-15.698-15.699-41.139,0-56.85   l105.54-105.549L11.774,68.611c-15.699-15.699-15.699-41.145,0-56.844c15.696-15.687,41.127-15.687,56.829,0l105.563,105.554   L279.721,11.767c15.705-15.687,41.139-15.687,56.832,0C352.258,27.466,352.258,52.912,336.559,68.611z" /></g>
                </svg>
            </a>

            <div class="todo-item__btn-complete" ref="btnComplete"></div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            dialogVisible: false
        }
    },
    props: {
        todo: {
            type: Object,
            required: true
        }
    },
    mounted() {
        this.$refs.btnComplete.addEventListener('mouseover', () => { 
            if (this.todo.done) {
                this.$refs.todo.classList.add('todo-item--uncomplete') 
            } else {
                this.$refs.todo.classList.add('todo-item--complete') 
            }
        }) 
        this.$refs.btnComplete.addEventListener('mouseleave', () => { 
            if (this.todo.done) {
                this.$refs.todo.classList.remove('todo-item--uncomplete') 
            } else {
                this.$refs.todo.classList.remove('todo-item--complete') 
            }
        }) 
    },
    methods: {
        updateTask() {
            const newTask = Object.assign({}, this.todo)
            newTask.done = !newTask.done

            this.$emit('update', newTask)
        },
        removeTodo() {
            this.$emit('removeTodo', this.todo.id)
        }
    }
}
</script>

<style scoped>
.todo-item {
    position: relative;
    display: flex;
    justify-content: space-between;
    width: 100%;
    margin: 10px 0;
    padding: 20px;
    border: 1px solid #282828;
    border-radius: 12px;
    cursor: pointer;
    overflow: hidden;
    word-break: break-word;
}
.todo-item--completed {
    background: linear-gradient(-45deg, #008000aa, #00800044, #07440733, #07440711, transparent, transparent, transparent, transparent, transparent, transparent, transparent);
}
.todo-item--complete {
    background: linear-gradient(-45deg, #008000aa, #00800011, transparent, transparent, transparent, transparent, transparent, transparent, transparent, transparent, transparent, transparent);
}
.todo-item--uncomplete {
    background: linear-gradient(-45deg, #008000aa, #00800044, #07440733, #07440711, transparent, transparent, transparent, #44070711, #44070733, #80000044, #800000aa);
}
.todo-item__title {
    font-size: 2em;
}
.todo-item__description {
    margin-top: 10px;
}.todo-item__title:hover, .todo-item__description:hover {
    cursor: text;
}
.todo-item__btn-remove {
    display: flex;
    justify-content: end;
    height: max-content;
    margin: -4px -8px 0 0;
    padding: 8px;
    fill: rgb(121, 121, 121);
    opacity: 0;
}
.todo-item__btn-remove:hover {
    fill: #bbbbbb;
}
.todo-item:hover .todo-item__btn-remove{
    opacity: 1;
}
.todo-item__btn-complete {
    position: absolute;
    bottom: 0;
    right: 0;
    width: 50px;
    height: 50px;
}
</style>
