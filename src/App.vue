<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <div class="app__btns">
 <my-button
    @click="showDialog"
   
    >
      Создать пост
    </my-button>
    <my-select
    v-model="selectedSort"
    :options="sortOptions"
    />
    </div>
    
   
   
    <my-dialog v-model:show="dialogVisible" ><h4>Создание поста</h4>
<post-form
   @create = "createPost"
  />
    </my-dialog>
  
  <post-list 
    :posts="sortedPosts"
    @remove="removePost"
    v-if="!isPostLoading"
   
    />
  <div v-else>Идет загрузка...</div>

   
   
  </div>
</template>

<script>

import PostList from '@/components/PostList.vue';
import PostForm from './components/PostForm.vue';
import MyButton from '@/components/UI/MyButton.vue';
import axios from 'axios';
import MySelect from '@/components/UI/MySelect.vue';

export default {
    components: {
    PostList,

    PostForm,
    MyButton, MySelect

},
  data() {
    return {
      posts: [ ],
    dialogVisible: false,
    modificatorValue:"" ,
    isPostLoading: false,
    selectedSort: '',
    sortOptions: [
      {value: 'title', name:'По названию'},
      {value: 'body', name:'По содержимому'},

    ]
    };
  },
  methods: {
    createPost(post) {
       this.posts.push(post);
       this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() { 
    this.dialogVisible = true;
    },
    async fetchPosts() {
      try {
        this.isPostLoading = true;
       
const response = await axios.get("https://jsonplaceholder.typicode.com/posts?_limit=10");
this.posts = response.data;

      
      } catch (e) {
        alert('Ошибка')
      } finally {
        this.isPostLoading = false;
      }
    }
  },
  mounted() {
    this.fetchPosts();
  },
computed:  {
  sortedPosts() {
    return [...this.posts].sort((post1,post2) =>  post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
  }
},

  watch: {
   
  }
  
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
.app__btns {
   margin: 15px 0;
  display: flex;
  justify-content: space-between;
}




</style>
