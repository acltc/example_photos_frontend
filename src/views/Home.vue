<template>
  <div class="home">
    <h1>New Photo</h1>
    <div>
      Name: <input type="text" v-model="newPhotoName" />
      Width: <input type="text" v-model="newPhotoWidth" />
      Height: <input type="text" v-model="newPhotoHeight" />
      Url: <input type="text" v-model="newPhotoUrl" />
      <button v-on:click="createPhoto()">Create Photo</button>
    </div>
    <h1>All Photos</h1>
    <div v-for="photo in photos">
      <h2>{{ photo.name }}</h2>
      <img v-bind:src="photo.url" v-bind:alt="photo.name" />
      <p>Width: {{ photo.width }}</p>
      <p>Height: {{ photo.height }}</p>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      photos: [],
      newPhotoName: "",
      newPhotoWidth: "",
      newPhotoHeight: "",
      newPhotoUrl: "",
    };
  },
  created: function() {
    this.indexPhotos();
  },
  methods: {
    indexPhotos: function() {
      axios.get("/api/photos").then(response => {
        console.log("photos index", response);
        this.photos = response.data;
      });
    },
    createPhoto: function() {
      var params = {
        name: this.newPhotoName,
        width: this.newPhotoWidth,
        height: this.newPhotoHeight,
        url: this.newPhotoUrl,
      };
      axios
        .post("/api/photos", params)
        .then(response => {
          console.log("photos create", response);
          this.photos.push(response.data);
          this.newPhotoName = "";
          this.newPhotoWidth = "";
          this.newPhotoHeight = "";
          this.newPhotoUrl = "";
        })
        .catch(error => {
          console.log("photos create error", error.response);
        });
    },
  },
};
</script>
