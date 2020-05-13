<template>
  <div class="photos-index">
    <h1>All Photos</h1>
    <div v-for="photo in photos">
      <h2>{{ photo.name }}</h2>
      <img v-bind:src="photo.url" v-bind:alt="photo.name" />
      <p>Width: {{ photo.width }}</p>
      <p>Height: {{ photo.height }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      photos: [],
    };
  },
  created: function() {
    axios.get("/api/photos").then(response => {
      console.log("photos index", response);
      this.photos = response.data;
    });
  },
  methods: {},
};
</script>
