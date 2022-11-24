<template>
  <main>

    <div class="heading">
      <h1>Infinite Scroll</h1>
      <h4>A simple infinite scroll example using Vue.js</h4>
    </div>

    <div class="container" id="app">

      <div class="list-group-wrapper">
        <transition name="fade">
          <div class="loading" v-show="loading">
            <span class="fa fa-spinner fa-spin"></span> Loading
          </div>
        </transition>
        <ul class="list-group" id="infinite-list">
          <li class="list-group-item" v-for="item in items" v-text="item"></li>
        </ul>
      </div>

    </div>

  </main>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      loading: false,
      items: []
    };
  },
  methods: {
    getNextBatch(size) {
      this.loading = true;
      axios.get("https://random-data-api.com/api/v2/users?size=" + size + "&response_type=json").then((response) => {
        
        response.data.map((x) => {
          console.log(x.first_name)
          this.items.push(x.first_name + " " + x.last_name) 
        })
        this.loading = false;
      });
    }
  },
  beforeMount() {
    this.getNextBatch(30);
  },
  mounted() {
    const listElm = document.querySelector('#infinite-list');

    listElm.addEventListener('scroll', e => {
      if (listElm.scrollTop + listElm.clientHeight >= listElm.scrollHeight) {
        this.getNextBatch(2);
      }
    })
  },
};
</script>

<style>

</style>


