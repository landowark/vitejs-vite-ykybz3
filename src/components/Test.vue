<script setup>
import { ref } from 'vue';
import { reactive, computed } from 'vue';
import axios from 'axios';

defineEmits(['selectEvent']);

defineProps({
  msg: String,
  seen: Boolean,
  attributeName: String,
  url: String,
});

const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery',
  ],
});

// a computed ref
const publishedBooksMessage = computed(() => {
  return author.books.length > 0 ? 'Yes' : 'No';
});

// const thing = computed(() => { axios
//   .get('https://jsonplaceholder.typicode.com/todos/')
//     .then((response) => {
//       // console.log(response.data);
//       return response.data;
//     })});
</script>

<template>
  <div class="card">
    <!-- emit event listened for by parent -->
    <button type="button" @click="$emit('selectEvent', msg)">Say Hello</button>
    <p v-if="seen">Now you see me</p>
    <br />
    <a v-bind:[attributeName]="url"> blah </a>
    <p>Has published books: {{ publishedBooksMessage }}</p>
    
  </div>
</template>
