<template>
  <div v-if="pageLoading" class="spinner-border" role="status">
    <span class="visually-hidden">Loading...</span>
  </div>

  <div v-else class="col-md-6">
    <h2 class="mb-5">Edit Post :</h2>
    <PostForm
      @formData="updatePost"
      :button-loading="loading"
      button-text="Edit Post"
      :post="post"
    />
  </div>
</template>

<script>
import PostForm from "@/components/posts/Form.vue";
import { ref } from "vue";
import axios from "axios";
import Swal from "sweetalert2";
import { useRoute } from "vue-router";
export default {
  components: {
    PostForm,
  },
  setup() {
    const loading = ref(false);
    const pageLoading = ref(true);
    const post = ref({});
    const route = useRoute();

    function getPost() {
      axios
        .get(`https://jsonplaceholder.typicode.com/posts/${route.params.id}`)
        .then(function (response) {
          post.value = response.data;
          pageLoading.value = false;
        })
        .catch(function (error) {
          console.log(error);
        })
        .finally(() => {
          pageLoading.value = false;
        });
    }
    getPost();

    function updatePost(formData) {
      loading.value = true;

      axios
        .put(`https://jsonplaceholder.typicode.com/posts/${route.params.id}`, {
          title: formData.title,
          body: formData.body,
          userId: route.params.id,
        })

        .then(function () {
          Swal.fire({
            title: "Thanks!",
            text: "Post update successfully",
            icon: "success",
            confirmButtonText: "Ok",
          });
        })
        .catch(function (error) {
          console.log(error);
        })
        .finally(() => {
          loading.value = false;
        });
    }

    return { updatePost, loading, post, pageLoading };
  },
};
</script>
