<template>
  <div class="watches-index">
    <h2> All Watches </h2> 
    
    
    <div v-for="watch in watches" class="card" style="width: 18rem;">
        <img v-bind:src="watch.img_url" class="card-img-top" alt="Card image cap">
        <div class="card-body">
          <p class="card-text">
            {{ watch.brand }} </p>
            {{ watch.model }}
        </div>
        <button v-on:click="deleteWatch(watch)"> Remove Watch </button> 
    </div>

    <h3> Is your watch not featured? Add one here </h3> 
    <div> 
        Model: 
        <input v-model="newWatchParams.model" type="text" />
        Brand: 
        <input v-model="newWatchParams.brand"type="text" />
        Category: 
        <input v-model="newWatchParams.category" type="text" />
        Price: 
        <input v-model="newWatchParams.price" type="text" />
        Image Link: 
        <input v-model="newWatchParams.img_url" type="text" /> 
        <button v-on:click="createWatch()"> Add Watch! </button> 
    </div> 
  </div>
</template>
<style>
img {
  max-width: 200px;
  max-height: 200px;
  padding: 2rem;
}

.card {
  display: inline-block;
  padding-right: 2rem;
}
</style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      watches: [],
      collectors: [],
      newWatchParams: {},
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
    createWatch: function () {
      axios
        .post("/watches", this.newWatchParams)
        .then((response) => {
          console.log("create watch", response);
          this.watches.push(response.data);
          this.newWatchParams = {};
        })
        .catch((error) => {
          console.log("watches error", error.response);
        });
    },
    deleteWatch: function (watch) {
      axios.delete("/watches/" + watch.id).then((response) => {
        console.log("deleted this watch", response);
        var index = this.watches.indexOf(watch);
        this.watches.splice(index, 1);
      });
    },
  },
};
</script>