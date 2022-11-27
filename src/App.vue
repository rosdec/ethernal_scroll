<template>
  <main>

    <div class="heading">
      <h1>Ethernal Scroll</h1>
      <h2>A simple infinite scroll example using Vue.js</h2>
      <h4>Pages loaded: {{ page_count }}</h4>
    </div>

    <div class="container" id="app">

      <div class="list-group-wrapper">
        <transition name="fade">
          <div class="loading" v-show="loading">
            <span class="fa fa-spinner fa-spin"></span> Loading next batch
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
      items: [],
      page_count: 0
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
        this.page_count++;
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
        this.getNextBatch(30);
      }
    })
  },
};
</script>

<style>

</style>


