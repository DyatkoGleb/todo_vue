<template>
    <div 
        class="todo-item"
    >
        <div 
            class="todo-item__content"
            :class="{'todo-item--completed': todo.done}"
            ref="todo"
        >
            <div>
                <!-- TODO: унифицировать форму создания CreateEditTodoForm и переиспользовать -->
                <my-input 
                    class="todo-item__title" 
                    v-model.trim="title" 
                    :inputValue="title" 
                    :inputError="titleError"
                ></my-input>
                <my-textarea 
                    class="todo-item__description" 
                    v-model.trim="description" 
                    :textareaValue="description" 
                    :descriptionError="descriptionError"
                ></my-textarea>
            </div>
            
            <div>
                <a class="todo-item__btn-remove" @click.stop="removeTodo">
                    <svg width="12px"  height="12px"  viewBox="0 0 348.333 348.334"  style="enable-background:new 0 0 348.333 348.334;">
                        <g><path d="M336.559,68.611L231.016,174.165l105.543,105.549c15.699,15.705,15.699,41.145,0,56.85   c-7.844,7.844-18.128,11.769-28.407,11.769c-10.296,0-20.581-3.919-28.419-11.769L174.167,231.003L68.609,336.563   c-7.843,7.844-18.128,11.769-28.416,11.769c-10.285,0-20.563-3.919-28.413-11.769c-15.699-15.698-15.699-41.139,0-56.85   l105.54-105.549L11.774,68.611c-15.699-15.699-15.699-41.145,0-56.844c15.696-15.687,41.127-15.687,56.829,0l105.563,105.554   L279.721,11.767c15.705-15.687,41.139-15.687,56.832,0C352.258,27.466,352.258,52.912,336.559,68.611z" /></g>
                    </svg>
                </a>

                <div class="todo-item__btn-complete" ref="btnComplete" @click="completeTask"></div>
            </div>
        </div>
        
        <div class="todo-item__btn-update-wrapper" @click="updateTask" v-if="needUpdate">
            <my-button>Update</my-button>
        </div>
    </div>
</template>

<script>
import MyInput from '@/components/UI/MyInput'
import MyTextarea from '@/components/UI/MyTextarea'
import MyButton from '@/components/UI/MyButton'

export default {
    components: { MyInput, MyTextarea, MyButton },
    data() {
        return {
            id: this.todo.id,
            done: this.todo.done,
            title: this.todo.title,
            description: this.todo.description,
            oldTitle: this.todo.title,
            oldDescription: this.todo.description,
            needUpdate: false,
            titleError: false,
            descriptionError: false
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
        completeTask() {
            this.done = !this.done

            this.updateTask()
        },
        updateTask() {
            const newTask = {
                id: this.id,
                title: this.title,
                description: this.description,
                done: this.done
            }
            
            this.$emit('update', newTask)

            this.needUpdate = false
            this.oldTitle = this.title
            this.oldDescription = this.description
        },
        removeTodo() {
            this.$emit('removeTodo', this.todo.id)
        },
        validData() {
            if (!this.title) { 
                this.titleError = true
                return false
            }
            this.titleError = false

            if (!this.description) { 
                this.descriptionError = true
                return false
            }
            this.descriptionError = false

            return true
        },
        isNeedUpdate() {
            if (!this.validData()) {
                return false
            }

            if (this.title != this.oldTitle || this.description != this.oldDescription) {
                return true
            } else {
                return false
            }
        },
        switchNeedUpdate() {
            if (this.isNeedUpdate()) {
                this.needUpdate = true
            } else {
                this.needUpdate = false
            }
        }
    },
    watch: {
        title() {
            this.switchNeedUpdate()
        },
        description() {
            this.switchNeedUpdate()
        }
    }
}
</script>

<style scoped>
.todo-item {
    width: 100%;
    margin: 10px 0;
    border: 1px solid #282828;
    border-radius: 12px;
    overflow: hidden;
    word-break: break-word;
    background: linear-gradient(-45deg, #020080aa, #02008044, #02008033, #02008011, #181818, #181818, #181818, #181818, #181818, #181818, #181818);
    overflow: hidden;
}
.todo-item__content {
    position: relative;
    display: flex;
    justify-content: space-between;
    margin: -1px -1px -1px -1px;
    padding: 20px;
    background-color: #181818;
    border: 1px solid #282828;
    border-radius: 12px;
}
.todo-item--completed {
    background: linear-gradient(-45deg, #095c09, #123212, #151f15, #171b17, #181818, #181818, #181818, #181818, #181818, #181818, #181818);
}
.todo-item--complete {
    background: linear-gradient(-45deg, #095c09, #171b17, #181818, #181818, #181818, #181818, #181818, #181818, #181818, #181818, #181818, #181818);
}
.todo-item--uncomplete {
    background: linear-gradient(-45deg, #095c09, #123212, #151f15, #171b17, #181818, #181818, #181818, #44070711, #44070733, #80000044, #800000aa);
}
.todo-item__title {
    font-size: 32px;
}
.todo-item__description {
    font-size: 16px;
    margin-top: 10px;
}
.todo-item__title, .todo-item__description {
    padding: 0;
    background: none;
    border: none;
    border-bottom: 1px solid transparent;
}
.todo-item__title:hover, .todo-item__description:hover {
    cursor: text;
}
.todo-item__title:focus, .todo-item__description:focus {
    border-bottom: 1px solid #eee;
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
    cursor: pointer;
}
.todo-item__btn-update-wrapper {
    display: flex;
    justify-content: end;
    padding: 10px 20px;
    border: none;
    cursor: pointer;
    transition: .2s;
}
</style>
