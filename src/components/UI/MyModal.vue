<template>
    <div class="modal" v-show="show" @click.stop="hideModal">
        <div @click.stop class="modal-content">
            <slot></slot>
        </div>
    </div>
</template>

<script>
    export default {
        name: "MyModal",
        props: {
            show: {
                type: Boolean,
                default: false
            }
        },
        methods: {
            hideModal() {
                this.$emit("update:show", false);
            }
        }
    }
</script>

<style scoped>
.modal{
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    position: fixed;
    display: flex;
    background: rgba(0,0,0, 0.5);
}

.modal-content{
    margin: auto;
    background: white;
    border-radius: 12px;
    min-width: 300px;
    min-height: 50px;
    padding: 20px;
}
</style>