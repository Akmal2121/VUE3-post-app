<template>
  <input type="text" class="input" :value="modelValue" @input="$emit('update:modelValue', $event.target.value)" />
</template>

<script>
export default {
  name: "MyInput",
  props: {
    modelValue: [String, Number],
  },
};
</script>

<style scoped>
.input {
  width: 100%;
  border: 1px solid gray;
  padding: 10px 15px;
  margin-top: 15px;
}
</style>
