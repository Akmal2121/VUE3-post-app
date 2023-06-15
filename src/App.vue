<template>
  <div class="app">
    <h2>Список Постов</h2>
    <MyInput
      v-model="searchInput"
      placeholder="Поиск..."
      style="margin-bottom: 10px"
    ></MyInput>
    <img :src="images[0].url" /><img />
    <div class="app_btns">
      <MyButton @click="showModal" class="create-post-btn"
        >Создать пост</MyButton
      >

      <MySelect :options="pageLimits" @change="opitionChange"></MySelect>
      <MySelect v-model="selectedSort" :options="options"></MySelect>
    </div>

    <MyModal v-model:show="modalVisible">
      <PostForm @get-data="saveData"></PostForm>
    </MyModal>
    <PostList :posts="sortedAndSearchedPosts" @remove="removeData"></PostList>

    <div class="pagination">
      <div
        v-for="pageNumber in totalPage"
        :key="pageNumber"
        class="page"
        :class="{ 'page-active': page === pageNumber }"
        @click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div>
    </div>
  </div>
</template>

<script>
import MyModal from "@/components/UI/MyModal.vue";
import MyButton from "@/components/UI/MyButton.vue";
import MySelect from "@/components/UI/MySelect.vue";
import MyInput from "@/components/UI/MyInput.vue";
import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";
import axios from "axios";

export default {
  components: {
    PostForm,
    PostList,
    MyModal,
    MyButton,
    MyInput,
    MySelect,
  },
  data: () => {
    return {
      url: "https://jsonplaceholder.typicode.com/posts",
      modalVisible: false,
      posts: [],
      images: [
        {
          id: 1,
          url: "../assets/img/img.jpg",
        },
      ],
      searchInput: "",
      selectedSort: "",
      options: [
        { value: "title", name: "По названию" },
        { value: "body", name: "По описанию" },
      ],
      pageLimits: [
        { value: 5, name: "Постов 5" },
        { value: 10, name: "Постов 10" },
        { value: 15, name: "Постов 15" },
        { value: 20, name: "Постов 20" },
      ],
      page: 1,
      limit: 5,
      totalPage: 0,
    };
  },
  methods: {
    showModal() {
      this.modalVisible = true;
    },
    async getPosts(page, limit) {
      try {
        if (page > 10 && limit == 10) {
          page = page / 2;
        } else if (page > 10 && limit == 15) {
          page = page / 3;
        } else if (page > 10 && limit == 20) {
          page = page / 4;
        }
        const response = await axios.get(this.url, {
          params: {
            _page: page,
            _limit: limit,
          },
        });
        this.totalPage = Math.ceil(response.headers["x-total-count"] / limit);
        this.posts = response.data;
      } catch (error) {
        console.log(error);
      }
    },
    saveData(data) {
      this.posts.push(data);
      this.modalVisible = false;
    },
    removeData(post) {
      this.posts = this.posts.filter((p) => p.id !== post.id);
    },
    changePage(pageNumber) {
      this.page = pageNumber;
      this.getPosts(this.page, this.limit);
    },
    opitionChange(param) {
      this.limit = param.target.value;
      this.getPosts(this.page, this.limit);
    },
  },
  computed: {
    sortedPosts() {
      return this.posts.sort((post1, post2) => {
        return post1[this.selectedSort]?.localeCompare(
          post2[this.selectedSort]
        );
      });
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter((post) => {
        const postTitle = post.title
          .toLowerCase()
          .includes(this.searchInput.toLowerCase());
        const postBody = post.body
          .toLowerCase()
          .includes(this.searchInput.toLowerCase());
        if (postTitle) {
          return postTitle;
        }
        return postBody;
      });
    },
  },
  mounted() {
    this.getPosts(this.page, this.limit);
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 20px;
}

.create-post-btn {
  margin: 15px 0;
}

.app_btns {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.pagination {
  display: flex;
  flex-wrap: wrap;
  margin-top: 15px;
  column-gap: 10px;
}

.page {
  padding: 10px;
  margin: 5px 0;
  text-align: center;
  width: 38px;
  height: 38px;
  border: 1px solid black;
  cursor: pointer;
}

.page-active {
  background-color: burlywood;
}
</style>
