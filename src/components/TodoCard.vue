<script setup>
import { ref, computed } from 'vue';


const props = defineProps({
  title: String,
  done: Boolean,
  id: Number,
  showDone: Boolean
});

const isActive = ref(false);
// Make local copy of done to persist through rerender
// Note, this doesn't change the status in any database however.
const complete = ref(props.done);

</script>

<template>
  
  <div v-bind:cardID="id" class="card" v-on:click="complete = !complete" :class="{ active: isActive }" v-show="showDone==complete" >
    <span>{{ title }}</span><span v-if="complete" style="font-weight:bold"> - Complete!</span>
  </div>
</template>
