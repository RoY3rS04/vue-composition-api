<script setup>
import { ref } from 'vue';

const header = ref('Shopping List App');
const items = ref([
  {id: 1, label: "10 party hats"},
  {id: 2, label: "2 board games"},
  {id: 3, label: "20 cups"},
]);

const newItem = ref('');
const newItemHighPriority = ref(false);

function addItem() {
  if (newItem.value === '') {
    return;
  }

  const newItemObject = {
    id: items.value.length + 1,
    label: newItem.value + (newItemHighPriority.value ? ' (High Priority)' : '')
  };

  items.value.push(newItemObject);
  newItem.value = '';
  newItemHighPriority.value = false;
}

</script>

<template>
  <h1>{{ header }}</h1>
  <form class='add-item-form' v-on:submit.prevent="addItem">
    <input v-model.trim="newItem" placeholder="Add an item">
    <label>
      <input type="checkbox" v-model="newItemHighPriority">
      High Priority
    </label>
    <button class="btn btn-primary">Save item</button>
  </form>
  <ul>
    <li v-for="item in items" :key="item.id">{{ item.label }}</li>
  </ul>
</template>