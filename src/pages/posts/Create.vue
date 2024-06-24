<template>
  <div class="col-md-6">
    <h2 class="mb-5">Create Post :</h2>

    <PostForm
      @formData="createPost"
      :button-loading="loading"
      button-text="Create Post"
    />
  </div>
</template>

<script>
import PostForm from "@/components/posts/Form.vue";
import axios from "axios";
import Swal from "sweetalert2";
import { ref } from "vue";
export default {
  components: {
    PostForm,
  },
  setup() {
    const loading = ref(false);
    function createPost(formData) {
      loading.value = true;
      axios
        .post("https://jsonplaceholder.typicode.com/posts", {
          title: formData.title,
          body: formData.body,
          userId: 1,
        })
        .then(function () {
          loading.value = false;
          Swal.fire({
            title: "Great",
            text: "Post created successfully",
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

    return { createPost, loading };
  },
};
</script>
