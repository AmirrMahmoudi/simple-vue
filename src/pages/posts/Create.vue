<template>
  <div class="col-md-6">
    <h2 class="mb-5">Create Post :</h2>

    <form @submit.prevent="validate">
      <div class="mb-3">
        <label class="form-label">Title :</label>
        <input
          type="text"
          class="form-control"
          v-model.lazy.trim="form.title"
        />
        <div id="emailHelp" class="form-text text-danger">
          {{
            form.titleErrorText && form.title === "" ? form.titleErrorText : ""
          }}
        </div>
      </div>
      <!-- Add a condition to hide the error message if the input is not empty -->
      <div class="mb-3">
        <label class="form-label">Body</label>
        <textarea class="form-control" v-model.lazy.trim="form.body"></textarea>
        <div id="emailHelp" class="form-text text-danger">
          <!-- Only show the error message if the input is empty -->
          {{ form.bodyErrorText && form.body === "" ? form.bodyErrorText : "" }}
        </div>
      </div>

      <button type="submit" class="btn btn-dark" :disabled="loading">
        <div
          v-if="loading"
          class="spinner-border spinner-border-sm"
          role="status"
        ></div>

        Create Post
      </button>
    </form>
  </div>
</template>

<script>
import { reactive, ref } from "vue";
import axios from "axios";
import Swal from "sweetalert2";
export default {
  setup() {
    const form = reactive({
      title: "",
      titleErrorText: "",
      body: "",
      bodyErrorText: "",
    });

    const loading = ref(false);

    function validate() {
      if (form.title === "") {
        form.titleErrorText = "Title is required";
      } else {
        form.titleErrorText = "";
      }
      if (form.body === "") {
        form.bodyErrorText = "Body is required";
      } else {
        form.bodyErrorText = "";
      }

      if (form.title !== "" && form.body !== "") {
        loading.value = true;
        createPost();
      }
    }
    function createPost() {
      axios
        .post("https://jsonplaceholder.typicode.com/posts", {
          title: form.title,
          body: form.body,
          userId: 1,
        })
        .then(function (response) {
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

    return { form, validate, loading };
  },
};
</script>
