<template>
  <b-container fluid class="home">
    <b-row class="mr-auto ml-auto mt-auto mb-auto">
      <b-col class="image-wrapper ml-auto mr-auto mt-auto mb-auto" cols="*">
        <img class="my-image" src="../assets/home/1.jpg">
      </b-col>
      <b-col class="text-wrapper ml-auto mr-auto mt-auto mb-auto" cols="*">
          <h1 class="my-text ml-auto mr-auto">
            {{contents[0].paragraph}}
          </h1>
          <p class="my-text ml-auto mr-auto" align="justify">
            {{contents[1].paragraph}}
          </p>
      </b-col>
    </b-row>
    <b-row>
      <h1 class="my-text ml-auto mr-auto">
        {{contents[2].paragraph}}
      </h1>
    </b-row>
    <b-row>
      <b-col sm="4" v-for="(video, index) in videos" :key="index">
        <iframe width="400" height="315" :src="video.paragraph" frameborder="0" allow="accelerometer; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
      </b-col>
    </b-row>
  </b-container>
</template>
<script>
import axios from 'axios';

  export default {
    data() {
      return {
        components: [],
        contents: [],
        videos: []
      };
    },
    methods: {
      getText() {
        const path = 'http://localhost:5000/Home';
        axios.get(path)
        .then((res) => {
          let components = res.data;
          let component;
          let duplicates = [];
          for(component of components){
            duplicates.push(component.component_number); 
          }
          this.contents = components;
          this.components = duplicates.filter((v, i, a) => a.indexOf(v) === i);
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.error(error);
        });
      },
      getVideos() {
        const path = 'http://localhost:5000/Video';
        axios.get(path)
        .then((res) => {
          this.videos = res.data;
          console.log(this.videos)
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.error(error);
        });
      },
    },
    created() {
      this.getText();
      this.getVideos()
    },
  };
</script>
<style>
  .my-image{
    border-radius: 10px;
    margin:20px 0px 20px 0px;
    box-shadow: -3px 3px 5px black;
    width: 100%;
  }
  .my-text{
    width: 90%;
  }
  .image-wrapper{
    width: 550px;
  }
  .text-wrapper{
    width: 602px;
  }
</style>