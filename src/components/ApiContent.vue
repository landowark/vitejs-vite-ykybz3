<script setup>

import axios from 'axios';
import {ref, computed} from 'vue';
import TodoCard from './TodoCard.vue';

const todos = await axios
  .get('https://jsonplaceholder.typicode.com/todos/')
  .then((response) => {
    console.log(response.data);
    return response.data.slice(0, 10);
  });

const showDone = ref(false);

</script>

<template>
  <h3>Todos</h3>
  <div>
    <button @click="showDone = !showDone"><span v-if="showDone">Done</span><span v-else>Pending</span></button>
    <TodoCard
      v-for="todo in todos"
      :title="todo.title"
      :done="todo.completed"
      :id="todo.id"
      :showDone="showDone"
    />
  </div>
</template>
