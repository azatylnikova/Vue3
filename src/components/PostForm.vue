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
        <my-input
          v-model="post.title"
          type="text"
          placeholder="Название"
        />
        <my-input
          v-model="post.body"
          type="text"
          placeholder="Описание"
        />
        <my-button class="btn" style="align-self: flex-end;margin-top: 15px;" @click="createPost">Создать</my-button>
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
       this.post.id = Date.now();
        this.$emit('create', this.post);
        this.post = {
          title: '',
          body: ''
     }
      }
    },
    // watch: {
    //  post: {
    //   handler(newValue){
    //     console.log(newValue , 'newValue')
    //   },
    //  deep: true
    //  }
    // }
  };
  </script>
  
  <style>
  form {
    display: flex;
    flex-direction: column;
  }

  </style>
  