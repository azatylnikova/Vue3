<template>
    <div class="dialog" v-if="show" @click.stop="hideDialog">
        <div @click.stop class="dialog__content">
            <slot></slot>
        </div>
    </div>
</template>

<script>
export default {
    name: 'my-dialog',
    props: {
        show : {
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
top: 0;
bottom: 0;
right: 0;
background: rgb(0, 0, 0, 0.5);
position: fixed;
display: flex;
width: 100%;
}
.dialog__content {
    margin: auto;
    background: white;
    border-radius: 12px;
    min-height: 58px;
    min-width: 300px;  
    padding: 20px;
}
</style>