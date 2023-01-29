<template>
    <textarea 
        ref="textarea"
        placeholder="Description" 
        maxlength="300"
        rows="1"
        :value="value"
		@input="updateInput"
    >
    </textarea>
</template>

<script>
export default {
    props: {
        textareaValue: { type: String },
        descriptionError: { type: Boolean }
    },
    data() {
        return {
            value: this.textareaValue
        }
    },
    mounted() {
        this.$refs.textarea.style.height = 0
        this.$refs.textarea.style.height = this.$refs.textarea.scrollHeight + 1 + 'px'
    },
	methods: {
		updateInput(event) {
            this.$refs.textarea.classList.remove('textarea-error')
            
            this.value = event.target.value
            
            this.autoGrow()
			this.$emit('update:modelValue', event.target.value)
		},
        autoGrow() {
            this.$refs.textarea.style.height = 0
            this.$refs.textarea.style.height = this.$refs.textarea.scrollHeight + 1 + 'px'
        },
    },
    watch: {
        descriptionError(isError) {
            if (isError) {
                this.$refs.textarea.classList.add('textarea-error')
            }
        }
    }
}
</script>

<style scoped>
.textarea-error {
    border-bottom: 1px solid #800000 !important;
}
textarea {
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
textarea {
    resize: none;
    overflow: hidden;
    margin-bottom: 10px;
}
textarea:hover, textarea:focus {
    border-bottom: 1px solid #eee;
}
textarea:hover::placeholder, textarea:focus::placeholder {
    color: #eee;
}
.error {
    border-bottom: 1px solid #800000;
}
</style>