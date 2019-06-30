<template>

  <div class="container">

    <h1>Latest Posts</h1>

    <div class="create-post">
      <label for="create-post"></label>
      <input type="text" id="create-post" v-model="text" placeholder="Create a post">
      <button v-on:click="createPost">Create Post</button>
    </div>

    <hr>

    <p class="error" v-if="error">{{error}}</p>

    <div class="posts-container">
      <div class="post"
           v-for="(post, index) in posts"
           v-bind:item="post"
           v-bind:index="index"
           v-bind:key="post._id"
      >

        {{`${post.createdAt.getDate()}/${post.createdAt.getMonth()}/${post.createdAt.getFullYear()}`}}

        <p class="text">{{post.text}}</p>

        <!--<button v-on:click="deletePost(post._id)">Delete Post</button>-->

      </div>
    </div>

  </div>
</template>

<script>

  import PostService from '../postService'

export default {
  name: 'PostComponent',
  data() {
    return {
      posts: [],
      error: '',
      text: ''
    }
  },
  async created() {
    try {

      this.posts = await PostService.getPosts()

    } catch(err) {

      this.err = err.message;

    }
  },

  methods: {
    async createPost() {
      await PostService.insertPost(this.text);
      this.posts = await PostService.getPosts();
    },

    async deletePost(id) {
      await PostService.deletePost(id);
      this.posts = await PostService.getPosts();
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
