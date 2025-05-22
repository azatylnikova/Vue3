<template>
    <div>
      <h1>Страница с постами</h1>
      <my-input v-model="searchQuery" placeholder="Поиск..." />
      <div class="app__btns">
        <my-button @click="showDialog"> Создать пост</my-button>
        <my-select v-model="selectedSort" :options="sortOptions" />
      </div>
  
      <my-dialog v-model:show="dialogVisible">
        <post-form @create="createPost" />
      </my-dialog>
  
      <post-list :posts="sortedAndSearchedPosts" @remove="removePost" v-if="!isPostLoading" />
      <div v-else>Идет загрузка...</div>
      <div ref="observer" class="observer"></div>
  
      <!-- <div class="page__wrapper">
        <div v-for="pageNumber in totalPages" 
        :key="pageNumber"
        class="page"
        :class="{
          'current-page': page === pageNumber
        }"
        @click="changePage(pageNumber)"
        > 
        {{ pageNumber }}
      </div>
      </div> -->
    </div>
  </template>
  <script>
  import PostForm from "@/components/PostForm.vue";
  import PostList from "@/components/PostList.vue";
  import MyDialog from "@/components/UI/MyDialog.vue";
  import axios from "axios";
  export default {
    components: {
      PostList,
      PostForm,
    },
    data() {
      return {
        posts: [],
        dialogVisible: false,
        isPostLoading: false,
        selectedSort: "",
        searchQuery: "",
        totalPages: 0,
        page: 1,
        limit: 10,
        sortOptions: [
          { value: "title", name: "По названию" },
          { value: "body", name: "По содержимому" },
        ],
      };
    },
    methods: {
      createPost(post) {
        this.posts.push(post);
        this.dialogVisible = false;
      },
      removePost(post) {
        this.posts = this.posts.filter((p) => p.id !== post.id);
      },
      showDialog() {
        this.dialogVisible = true;
      },
      // changePage(pageNumber) {;
      //    this.page = pageNumber;
      // },
      async fetchPosts() {
        try {
          this.isPostLoading = true;
          const response = await axios.get(
            "https://jsonplaceholder.typicode.com/posts", {
            params: {
              _page: this.page,
              _lomit: this.limit
            }
          }
          );
          this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
          this.posts = response.data;
          this.isPostLoading = false;
        } catch (e) {
          alert("Ошибка");
        }
      },
      async loadMorePosts() {
        try {
          this.page += 1;
          // this.isPostLoading = true;
          const response = await axios.get(
            "https://jsonplaceholder.typicode.com/posts", {
            params: {
              _page: this.page,
              _lomit: this.limit
            }
          }
          );
          this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
          this.posts = [...this.posts, ...response.data];
          //  this.isPostLoading = false;
        } catch (e) {
          alert("Ошибка");
        }
      },
    },
  
    mounted() {
      this.fetchPosts();
      console.log(this.$refs)
      const options = {
        rootMargin: "0px",
        threshold: 1.0,
      };
      const callback = (entries, observer) => {
        if (entries[0].isIntersecting && this.page < this.totalPages ) {
          this.loadMorePosts()
        }
      };
      const observer = new IntersectionObserver(callback, options);
      observer.observe(this.$refs.observer);
    },
    /// на уровне vue используется не как ф-я а как обычную переменную
    computed: {
      sortedPosts() {
        return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
      },
  
      sortedAndSearchedPosts() {
        return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
      }
    },
  
    /////// ф-я наблюдатель должна иметь такое же название как и модель , которая обьявлена в компоненте
    // наблюдатель , который следит за изменением св-ва selectedSort
    //   watch: {
    // selectedSort(newValue) {
    //  this.posts.sort( (post1, post2) => {
    //   return post1[newValue]?.localeCompare(post2[newValue])
    //  })
    // }
    //   }
  
    watch: {
      // page() {
      //   this.fetchPosts();
      // }
    }
  };
  </script>
  
  <style>

  
  .app__btns {
    margin: 15px 0;
    display: flex;
    justify-content: space-between;
  }
  
  .page__wrapper {
    display: flex;
    margin-top: 15px;
  }
  
  .page {
    border: 1px solid black;
    padding: 10px;
  }
  
  .current-page {
    border: 2px solid teal;
  }
  
  .observer {
    height: 30px;
    background: green;
  
  }
  </style>
  