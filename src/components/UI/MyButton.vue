<template>
  <button class="btn" type="submit">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "MyButton",
};
</script>

<style scoped>
.btn {
  border: 1px solid gray;
  color: gray;
  padding: 10px 15px;
  background: none;
  cursor: pointer;
}
</style>
