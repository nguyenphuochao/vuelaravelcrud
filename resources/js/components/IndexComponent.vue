// IndexComponent.vue

<template>
  <div>
    <h1>Posts</h1>
    <div class="row">
      <div class="col-md-10"></div>
      <div class="col-md-2">
        <router-link :to="{ name: 'create' }" class="btn btn-primary"
          >Create Post</router-link
        >
      </div>
    </div>
    <br />

    <h1 v-if="loading">Loading...</h1>

    <table class="table table-hover" v-else>
      <thead>
        <tr>
          <th>ID</th>
          <th>Item Name</th>
          <th>Item Price</th>
          <th colspan="2">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="post in posts" :key="post.id">
          <td>{{ post.id }}</td>
          <td>{{ post.title }}</td>
          <td>{{ post.body }}</td>
          <td>
            <router-link
              :to="{ name: 'edit', params: { id: post.id } }"
              class="btn btn-primary"
              >Edit</router-link
            >
          </td>
          <td><button class="btn btn-danger" @click.prevent="handleDeletePost(post.id)">Delete</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loading: true,
      posts: [],
    };
  },
  created() {
    let uri = "http://127.0.0.1:8000/api/posts";
    this.axios.get(uri).then((response) => {
      this.loading = false;
      this.posts = response.data.data;
    });
  },
  methods: {
    handleDeletePost(id) {
        let uri = "http://127.0.0.1:8000/api/post/delete/" + id;
        this.axios.delete(uri).then((res) => {
            this.posts.splice(this.posts.indexOf(id), 1);
        });
    }
  }
};
</script>
