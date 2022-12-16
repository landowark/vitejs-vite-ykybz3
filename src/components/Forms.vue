<script setup>
import { ref } from 'vue';
import { reactive, computed } from 'vue';

const message = ref('')
const checked = ref(true)
const checkedNames = ref([])
const picked = ref('One')
const selected = ref([])

const options = ref([
  { text: 'One', value: 'A' },
  { text: 'Two', value: 'B' },
  { text: 'Three', value: 'C' }
])

function makeText(filename, text) {
		var element = document.createElement('a');
		element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(text));
		element.setAttribute('download', filename);
		element.innerHTML = "Download " + filename;
		element.style.display = 'none';
		document.body.appendChild(element);
		element.click();
		document.body.removeChild(element);
	};

</script>

<template>
<form @submit="makeText(checkedNames.join(' '), message)">
<p>Message is: {{ message }}</p>
<input v-model.trim="message" placeholder="edit me" />

<br/><br/>

<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<textarea v-model="message" placeholder="add multiple lines"></textarea>

<br/><br/>

<input type="checkbox" id="checkbox" v-model="checked" />
<label for="checkbox">{{ checked.toString()[0].toUpperCase() + checked.toString().slice(1) }}</label>

<br/><br/>

<div>Checked names: {{ checkedNames }}</div>
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
<label for="jack">Jack</label>
<input type="checkbox" id="john" value="John" v-model="checkedNames">
<label for="john">John</label>
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label>

<br/><br/>

<div>Picked: {{ picked }}</div>
<input type="radio" id="one" value="One" v-model="picked" />
<label for="one">One</label>
<input type="radio" id="two" value="Two" v-model="picked" />
<label for="two">Two</label>

<br/><br/>

<div>Selected: {{ selected }}</div>
<select v-model="selected">
  <option disabled value="">Please select one</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>

<br/><br/>

<div>Selected: {{ selected }}</div>
<select v-model="selected" multiple>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>

<br/><br/>

<select v-model="selected">
  <option v-for="option in options" :value="option.value">
    {{ option.text }}
  </option>
</select>
<div>Selected: {{ selected }}</div>

<br/><br/>

<button type="submit" value="Submit" >Submit</button>

<br/><br/>

</form>

</template>