<template>
  <div class="home">
    <h2> Current Collectors </h2> 
    <div v-for="collector in collectors" class="card" style="width: 18rem;">
      <img v-bind:src="collector.img_url" class="card-img-top" alt="Card image cap">
      <div class="card-body">
  
        <p class="card-text"> {{ collector.name }}
          {{ collector.age }} </p>
        <p class="card-text"> {{ collector.email }} </p> 
        <!-- <div v-for="watch in collector.watches" class="card-text"> 
           {{ watch.category }}
        </div>  -->
  
        <button v-on:click="viewCollector(collector)"> View More </button> 
        <button v-on:click="deleteCollector(collector)"> Remove Collector </button> 
      </div>
    
      <dialog id="collector-details"> 
        <form method="dialog"> 
          <h1> Collector Information </h1> 
          <p> {{ currentCollector.name }} </p> 
          <p> {{ currentCollector.email }} </p> 
          <p> {{ currentCollector.age }} </p>
          <h2>  Edit Collector Information </h2>
          Name: 
          <input type="text" v-model="currentCollector.name" />  
          Age: 
          <input type="text" v-model="currentCollector.age" /> 
          Email: 
          <input type="text" v-model="currentCollector.email" />   
          Image: 
          <input type="text" v-model="currentCollector.img_url" />
          <div v-for="watch in currentCollector.watches"> 
              <p>  {{ watch }} </p> 
          </div> 
          <button v-on:click="editCollector()"> Update Collector </button>   
          <button> Close </button> 
        </form> 
      </dialog> 
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
  max-width: 100px;
  max-height: 100px;
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
      currentCollector: {},
      editCollectorParams: {},
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
    viewCollector: function (collector) {
      this.currentCollector = collector;
      document.querySelector("#collector-details").showModal();
    },
    editCollector: function (collector) {
      var editCollectorParams = collector;
      axios
        .patch("/collectors/" + collector.id, editCollectorParams)
        .then((response) => {
          console.log("edit Collector", response);
          this.currentCollector = {};
        })
        .catch((error) => {
          console.log("update Collector error", error.response);
        });
    },
    deleteCollector: function (collector) {
      axios.delete("/collectors/" + collector.id).then((response) => {
        console.log("destroy the collector!!!", response);
        var index = this.collectors.indexOf(collector);
        this.collectors.splice(index, 1);
      });
    },
  },
};
</script>