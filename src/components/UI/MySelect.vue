<template>
  <select :v-model="modelValue" @change="changeOption">
    <option disabled value="">Выберете из списка</option>
    <option v-for="option in options" :key="option.value" :value="option.value">
      {{ option.name }}
    </option>
  </select>
</template>

<script>
export default {
  name: "MySelect",
  props: {
    modelValue: {
      type: String,
    },
    options: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    changeOption(event) {
      this.$emit("update:modelValue", event.target.value);
    },
  },
};
</script>

<style scoped>
select {
  border: 1px solid gray;
  color: black;
  outline: none;
  padding: 10px;
}

select:focus {
  outline: none;
}
</style>
