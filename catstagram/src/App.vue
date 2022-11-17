<template>
  <div id="app">
    <div class="app-phone card">
      <div class="text-center mt-2">
        <img class="card-img-top" src="./components/icons/Catstagram.png" />
        <a class="cancel-cta" v-if="step === 2 || step === 3" @click="goToHome"
          >Cancel</a
        >
        <a class="next-cta" v-if="step === 2" @click="sharePost">Share</a>
      </div>
      <div class="card-body">
        <app-body
          :step="step"
          :posts="posts"
          :image="image"
          v-model="caption"
        />
      </div>
      <div class="phone-footer">
        <div class="home-cta" @click="goToHome">
          <img class="img-fluid" src="./components/icons/cat-solid.png" />
        </div>
        <div class="upload-cta">
          <input
            type="file"
            name="file"
            id="file"
            class="inputfile"
            @change="uploadImage"
            :disabled="step !== 1"
          />
          <label for="file"><i class="fa fa-plus-square fa-lg"></i></label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AppBody from "./components/AppBody.vue";

import posts from "./data/posts";

export default {
  name: "App",
  data() {
    return {
      step: 1,
      posts,
      image: "",
      caption: "",
    };
  },
  methods: {
    uploadImage(evt) {
      const files = evt.target.files;
      if (!files.length) return;

      const reader = new FileReader();
      reader.readAsDataURL(files[0]);
      reader.onload = (evt) => {
        this.image = evt.target.result;
        this.step = 2;
      };

      // To enable reuploading of same files in Chrome
      document.querySelector("#file").value = "";
    },
    goToHome() {
      this.image = "";
      this.caption = "";
      this.step = 1;
    },
    sharePost() {
      const post = {
        username: "Faudrey",
        userImage:
          "https://s3-us-west-2.amazonaws.com/s.cdpn.io/1211695/vue_lg_bg.png",
        image: this.image,
        likes: 0,
        caption: this.caption,
      };
      this.posts.unshift(post);
      this.goToHome();
      console.log(this.caption);
    },
  },
  components: {
    "app-body": AppBody,
  },
};
</script>

<style lang="css" src="./assets/main.css"></style>
