<template>
  <main>
    <div id="app">
      <h1>Ethernal scroll of Random users</h1>
      <div id="avatars">
        <div class="user" v-for="user in users" :key="user.first_name">
          <div class="user-avatar">
            <img :src="user.avatar" />
          </div>
          <div class="user-details">
            <h2 class="user-name">
              {{ user.first_name }}
              {{ user.last_name }}
            </h2>
            <ul>
              <li><strong>Birthday:</strong> {{ formatDate(user.date_of_birth) }}</li>
              <li><strong>Location:</strong> {{ user.address.city }}, {{ user.address.state }}</li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      users: [],
    };
  },
  methods: {
    getInitialUsers() {
      axios.get(`https://random-data-api.com/api/v2/users?size=6&response_type=json`).then((response) => {
        this.users = response.data;
        console.log(response.data)
      });
    },
    formatDate(dateString) {
      let convertedDate = new Date(dateString);
      return convertedDate.toDateString();
    },
    getNextUser() {
      window.onscroll = () => {
        let bottomOfWindow = document.documentElement.scrollTop + window.innerHeight === document.documentElement.offsetHeight;
        if (bottomOfWindow) {
          axios.get(`https://random-data-api.com/api/v2/users?size=2&response_type=json`).then(response => {
            this.users.push(response.data[0]);
            this.users.push(response.data[1]);
          });
        }
      }
    },
  },
  beforeMount() {
    this.getInitialUsers();
  },
  mounted() {
    this.getNextUser();
  },
};
</script>

<style>
.user {
  display: flex;
  background: #ccc;
  border-radius: 0.5em;
  margin: 1em auto;
}

.user-avatar {
  padding: 1em;
}

.user-avatar img {
  display: block;
  width: 60%;
  min-width: 64px;
  height: auto;
  border-radius: 50.5em;
}

.user-details {
  padding: 1em;
}

.user-name {
  margin: 0;
  padding: 0;
  font-size: 1rem;
  font-weight: 900;
}
</style>


