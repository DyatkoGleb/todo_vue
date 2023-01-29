<template>
    <input 
        v-focus
        ref="input"
        type="text" 
        placeholder="Title"
        :value="value"
		@input="updateInput"
    >
</template>

<script>
export default {
    props: {
        inputValue: { type: String },
        inputError: { type: Boolean }
    },
    data() {
        return {
            value: this.inputValue
        }
    },
	methods: {
		updateInput(event) {
            this.$refs.input.classList.remove('inputError')
            this.value = event.target.value
			this.$emit('update:modelValue', event.target.value)
		}
    },
    watch: {
        inputError(isError) {
            if (isError) {
                this.$refs.input.classList.add('inputError')
            }
        }
    }
}
</script>

<style scoped>
.inputError {
    border-bottom: 1px solid #800000 !important;
}
input {
    display: block;
    width: 100%;
    margin-bottom: 13px;
    background: none;
    border: none;
    border-bottom: 1px solid #282828;
    font-size: 14px;
    resize: none;
    height: max-content;
    overflow: hidden;
}
input:hover, input:focus {
    border-bottom: 1px solid #eee;
}
input:hover::placeholder, input:focus::placeholder {
    color: #eee;
}
.error {
    border-bottom: 1px solid #800000;
}
</style>