<template>
  <!-- <h1>{{ users }}</h1> -->

  <div v-if="loading" class="spinner-border" role="status">
    <span class="visually-hidden">Loading...</span>
  </div>

  <div v-else class="col-md-5">
    <div class="card">
      <div class="card-header">
        <!-- <router-link :to="{ name: 'postId', params: { id: post.id } }"> -->
        {{ post.title }}
        <!-- </router-link> -->
      </div>
      <ul class="list-group list-group-flush">
        <li class="list-group-item">Body : {{ post.body }}</li>
      </ul>
      <div class="card-footer">
        <button @click="deletePost" class="btn btn-sm btn-danger me-4">
          Delete
        </button>
        <router-link :to="{ name: 'editPost', params: { id: post.id } }">
          <button class="btn btn-sm btn-dark">Edit</button>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { ref } from "vue";
import { useRoute } from "vue-router";
import Swal from "sweetalert2";

export default {
  setup() {
    const post = ref({});
    const loading = ref(true);
    const route = useRoute();

    function getPost() {
      axios
        .get(`https://jsonplaceholder.typicode.com/posts/${route.params.id}`)
        .then(function (response) {
          post.value = response.data;
          loading.value = false;
        })
        .catch(function (error) {
          console.log(error);
        });
    }
    getPost();

    function deletePost() {
      axios
        .delete(`https://jsonplaceholder.typicode.com/posts/${route.params.id}`)
        .then(function () {
          Swal.fire({
            title: "Deleted",
            text: `Post (${route.params.id}) deleted successfully`,
            icon: "warning",
            confirmButtonText: "Ok",
          });
        })
        .catch(function (error) {
          console.log(error);
        });
    }

    return { post, loading, route, deletePost };
  },
};
</script>
