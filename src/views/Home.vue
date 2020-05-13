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
      <button v-on:click="showPhoto(photo)">More info</button>
    </div>
    <dialog id="photo-details">
      <form method="dialog">
        <h1>Photo info</h1>
        <p>Name: <input type="text" v-model="currentPhoto.name" /></p>
        <p>Width: <input type="text" v-model="currentPhoto.width" /></p>
        <p>Height: <input type="text" v-model="currentPhoto.height" /></p>
        <p>Url: <input type="text" v-model="currentPhoto.url" /></p>
        <button v-on:click="updatePhoto(currentPhoto)">Update</button>
        <button>Close</button>
      </form>
    </dialog>
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
      currentPhoto: {},
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
    showPhoto: function(photo) {
      this.currentPhoto = photo;
      document.querySelector("#photo-details").showModal();
    },
    updatePhoto: function(photo) {
      var params = {
        name: photo.name,
        width: photo.width,
        height: photo.height,
        url: photo.url,
      };
      axios
        .patch("/api/photos/" + photo.id, params)
        .then(response => {
          console.log("photos update", response);
          this.currentPhoto = {};
        })
        .catch(error => {
          console.log("photos update error", error.response);
        });
    },
  },
};
</script>
