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
  },
};
</script>