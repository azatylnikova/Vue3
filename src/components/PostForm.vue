<template>
      <form @submit.prevent>
        <!-- <input
          v-bind:value="title"
          @input="post.title = $event.target.value"
          class="input"
          type="text"
          placeholder="Название"
        />
        <input
          v-bind:value="body"
          @input="post.body = $event.target.value"
          class="input"
          type="text"
          placeholder="Описание"
        /> -->
        <input
          v-model="post.title"
          class="input"
          type="text"
          placeholder="Название"
        />
        <input
          v-model="post.body"
          class="input"
          type="text"
          placeholder="Описание"
        />
        <button class="btn" @click="createPost">Создать</button>
      </form>
  </template>
  <script>
  export default {
    data() {
      return {
        post: {
          title: '',
          body: ''
        }
      }
    },
    methods: {
      createPost() {
        this.post.id = Date.new();
        this.posts.push(newPost);
        this.title= '';
        this.body='';
      }
    }
  };
  </script>
  
  <style>
  form {
    display: flex;
    flex-direction: column;
  }
  .btn {
    align-self: flex-end;
    margin-top: 15px;
    padding: 10px 15px;
    background: none;
    color: teal;
    border: 1px solid teal;
  }
  .input {
    width: 100%;
    border: 1px solid teal;
    padding: 10px 15px;
    margin-top: 15px;
    border: 1px solid teal;
  }
  </style>
  