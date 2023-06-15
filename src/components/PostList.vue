<template>
  <transition-group name="post-list">
    <PostItem v-for="post in posts" :post="post" @remove="$emit('remove', post)"></PostItem>
  </transition-group>
</template>

<script>
import PostItem from "./PostItem.vue";
export default {
  name: "PostList",
  components: {
    PostItem,
  },

  props: {
    posts: {
      type: Array,
      default: [],
      required: true,
    },
  },
};
</script>

<style scoped>
.post-list-item {
  display: inline-block;
  margin-right: 10px;
}

.post-list-enter-active,
.post-list-leave-active {
  transition: all 1.4s ease;
}

.post-list-enter-from,
.post-list-leave-to {
  opacity: 0;
  transform: translateX(130px);
}

.post-list-move {
  transition: transform 1.4s ease;
}
</style>
