<template>
  <div class="app">
    <h4>Создание поста</h4>
  <PostForm
   @create = "createPost"
  />
  <PostList 
    :posts="posts"
    @remove="removePost"
   
    />

   
   
  </div>
</template>

<script>

import PostList from '@/components/PostList.vue';
import PostForm from './components/PostForm.vue';

export default {
    components: {
    PostList,

    PostForm
},
  data() {
    return {
      posts: [
        { id: 1, title: "JS", body: "Оисание поста" },
        { id: 2, title: "JS 2", body: "Оисание поста 2" },
        { id: 3, title: "JS 3", body: "Оисание поста 3" },
      ],
    
    };
  },
  methods: {
    createPost(post) {
       this.posts.push(post);
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    }
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




</style>
