<template>
  <form @submit.prevent="addPost">
    <h2>Создание поста</h2>
    <MyInput v-model="title" placeholder="Название"></MyInput>
    <MyInput v-model="body" placeholder="Описание"></MyInput>
    <MyButton class="create-btn">Создать</MyButton>
  </form>
</template>

<script>
import MyButton from "./UI/MyButton.vue";
import MyInput from "./UI/MyInput.vue";

export default {
  name: "PostForm",
  components: {
    MyButton,
    MyInput,
  },
  data: () => {
    return {
      title: "",
      body: "",
    };
  },
  methods: {
    addPost() {
      const obj = {
        id: Date.now(),
        title: this.title,
        body: this.body,
      };
      this.$emit("get-data", obj);
      this.title = '';
      this.body= '';
    },
  },
};
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}

.create-btn {
  margin-top: 15px;
  align-self: flex-end;
}
</style>
