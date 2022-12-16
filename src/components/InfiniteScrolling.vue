<!-- https://www.digitalocean.com/community/tutorials/vuejs-implementing-infinite-scroll -->
<script setup>
import { ref, onBeforeMount, onMounted, reactive } from 'vue';
import axios from 'axios';

// The reactive part is necessary for adding users later
var users = reactive(await axios.get(`https://randomuser.me/api/?results=5`).then((response) => {
        return response.data.results;
    }));

function formatDate(dateString) {
      let convertedDate = new Date(dateString);
      return convertedDate.toDateString();
    }

window.onscroll = () => {
  // let bottomOfWindow = document.documentElement.scrollTop + window.innerHeight === document.documentElement.offsetHeight;
  let bottomOfWindow = window.innerHeight + window.scrollY >= document.body.offsetHeight;
  console.log(bottomOfWindow);
  if (bottomOfWindow) {
    console.log("Hit the bottom!");
    axios.get(`https://randomuser.me/api/?results=3`).then(response => {
      users.push(...response.data.results);
      // console.log(users)
    });
  }
};

</script>

<template>
  <h1>Random User</h1>
  <div class="user" v-for="user in users" :key="user.first">
    <div class="user-avatar">
      <img :src="user.picture.large" />
    </div>
    <div class="user-details">
      <h2 class="user-name">
        {{ user.name.first }}
        {{ user.name.last }}
      </h2>
      <ul>
        <li><strong>Birthday:</strong> {{ formatDate(user.dob.date) }}</li>
        <li>
          <strong>Location:</strong> {{ user.location.city }},
          {{ user.location.state }}
        </li>
      </ul>
    </div>
  </div>
</template>

<style>
.user {
  display: flex;
  background: #ccc;
  border-radius: 1em;
  margin: 1em auto;
}

.user-avatar {
  padding: 1em;
}

.user-avatar img {
  display: block;
  width: 100%;
  min-width: 64px;
  height: auto;
  border-radius: 50%;
}

.user-details {
  padding: 1em;
}

.user-name {
  margin: 0;
  padding: 0;
  font-size: 2rem;
  font-weight: 900;
}
</style>
