<template>
  <div class="home">
    <h2> Current Collectors </h2> 
    <div v-for="collector in collectors" class="card" style="width: 18rem;">
      <img v-bind:src="collector.img_url" class="card-img-top" alt="Card image cap">
      <div class="card-body">
        <p class="card-text"> {{ collector.name }}
          {{ collector.age }} </p>
        <p class="card-text"> {{ collector.email }} </p> 
      </div>
    </div>
         <h3> Know a Collector? Add one!! </h3> 

         <div> 
            Name: 
            <input type="text" v-model="newCollectorParams.name" /> 
            Email: 
            <input type="text" v-model="newCollectorParams.email" /> 
            Age: 
            <input type="text" v-model="newCollectorParams.age" /> 
            Image: 
            <input type="text" v-model="newCollectorParams.img_url" /> 
            <button v-on:click="createCollector()"> Add a Collector Profile </button> 
          </div> 
    </div>


</template>
<style>
img {
  max-width: 200px;
  max-height: 200px;
}
</style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      watches: [],
      collectors: [],
      newCollectorParams: {},
    };
  },
  created: function () {
    this.indexWatches();
    this.indexCollectors();
  },
  methods: {
    indexWatches: function () {
      axios.get("/watches").then((response) => {
        console.log("watches index", response);
        this.watches = response.data;
      });
    },
    indexCollectors: function () {
      axios.get("/collectors").then((response) => {
        console.log("collectors index", response);
        this.collectors = response.data;
      });
    },
    createCollector: function () {
      axios
        .post("/collectors", this.newCollectorParams)
        .then((response) => {
          console.log("make a collector!", response);
          this.collectors.push(response.data);
          this.newCollectorParams = {};
        })
        .catch((error) => {
          console.log("new collector error", error.response);
        });
    },
  },
};
</script>