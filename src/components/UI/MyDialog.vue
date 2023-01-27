<template>
    <div class="dialog" v-if="show" @click="hideDialog">
        <my-form-wrapper class="dialog__content" @click.stop>
            <slot></slot>
        </my-form-wrapper>
    </div>
</template>

<script>
import MyFormWrapper from '@/components/UI/MyFormWrapper'

export default {
    components: { MyFormWrapper },
    props: {
        show: {
            type: Boolean,
            default: false
        }
    },
    methods: {
        hideDialog() {
            this.$emit('update:show', false)
        }
    }
}
</script>

<style scoped>
.dialog {
    position: fixed;
    display: flex;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    background-color: #0008;
}
.dialog__content {
    min-width: 350px;
    max-width: 500px;
    margin: auto;
    background-color: #181818;
    overflow: hidden;
    border-radius: 12px;
}
</style>