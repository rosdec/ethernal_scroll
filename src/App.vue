<template>
  <main>

    <div class="heading">
      <h1>Ethernal Scroll</h1>
      <h2>A simple infinite scroll example using Vue.js</h2>
      <h4>Pages loaded: {{ page_count }}</h4>
    </div>

    <div class="container" id="app">
      <grid-container id="infinite-list">

        <transition name="fade">
          <div class="loading" v-show="loading">
            Loading next batch
          </div>
        </transition>

        <grid-item :class="item.size" v-for="item in items">
          <p><em>Name</em>: {{ item.name }}<br>
            <em>Date of birth</em>: {{ item.dob }}<br>
            <em>Place of birth</em>: {{ item.location }}</p>
        </grid-item>
      </grid-container>
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
      page_count: 0,
      sizes: ["tall", "taller", "tallest"]
    };
  },
  methods: {
    getNextBatch(size) {
      this.loading = true;
      axios.get("https://random-data-api.com/api/v2/users?size=" + size + "&response_type=json").then((response) => {

        response.data.map((x) => {
          const new_item = {
            name: x.first_name + " " + x.last_name,
            dob: x.date_of_birth,
            location: x.address.state,
            size: this.sizes[Math.floor(Math.random() * this.sizes.length)]
          };
          this.items.push(new_item)
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
    const masonry = document.querySelector('#infinite-list');

    masonry.addEventListener('scroll', e => {
      if (masonry.scrollTop + masonry.clientHeight >= masonry.scrollHeight) {
        this.getNextBatch(30);
      }
    })
  },
};
</script>

<style>
body {
  background-color: #ffffff;
  padding: 50px;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif
}

.container {
  padding: 40px 80px 15px 80px;
  background-color: #fff;
  border-radius: 8px;
  max-width: 800px;
  margin: auto;
}

.heading {
  text-align: center;
}

.heading h1 {
  text-align: center;
  margin: 0 0 5px 0;
  font-weight: 900;
  font-size: 4rem;
  color: #333;
}

.heading h4 {
  color: #555;
  text-align: center;
  margin: 0 0 35px 0;
  font-weight: 400;
  font-size: 24px;
}

.loading {
  text-align: center;
  position: absolute;
  color: #fff;
  z-index: 100;
  background: #0dd85b;
  padding: 8px 18px;
  border-radius: 5px;
  left: calc(50% - 45px);
  top: calc(50% - 18px);
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}

grid-container {
  display: grid;
  /* 1 */
  grid-auto-rows: 50px;
  /* 2 */
  grid-gap: 10px;
  /* 3 */
  grid-template-columns: repeat(auto-fill, minmax(30%, 1fr));
  /* 4 */
  overflow: auto;
  height: 30vh;
  border: 2px solid;
  border-radius: 5px;
  text-align: center;
  margin: 0 0 35px 0;
}

.tall {
  grid-row: span 2;
  background-color: #7632ed;
}

.taller {
  grid-row: span 3;
  background-color: #925cef;
}

.tallest {
  grid-row: span 4;
  background-color: #b9a3de;
}
</style>


