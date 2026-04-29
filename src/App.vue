<script setup>
import { ref, computed } from 'vue';

const header = ref('Shopping List App');
const items = ref([
  {id: 1, label: "10 party hats", purchased: true, highPriority: false},
  {id: 2, label: "2 board games", purchased: false, highPriority: false},
  {id: 3, label: "20 cups", purchased: false, highPriority: true},
]);

const reversedItems = computed(() => {
  return [...items.value].reverse();
});

const editing = ref(false);

const characterCount = computed(() => {
  return newItem.value.length;
});

const newItem = ref('');
const newItemHighPriority = ref(false);

function addItem() {
  if (newItem.value === '') {
    return;
  }
    
  const newItemObject = {
    id: items.value.length + 1,
    label: newItem.value,
    highPriority: newItemHighPriority.value,
  };

  items.value.push(newItemObject);
  newItem.value = '';
  newItemHighPriority.value = false;
}

function togglePurchased(id) {

  const item = items.value.find(item => item.id === id);
  if (item) {
    item.purchased = !item.purchased;
  }
}

const doEdit = (e) => {
  editing.value = e;
  newItem.value = '';
  newItemHighPriority.value = false;
}

</script>

<template>
  <header class="header">
    <h1>{{ header }}</h1>
    <button v-if="editing" @click="doEdit(false)" class="btn">Cancel</button>
    <button v-else @click="doEdit(true)" class="btn btn-primary">Add Item</button>
  </header>
  <form v-if="editing" class='add-item-form' @submit.prevent="addItem">
    <input v-model.trim="newItem" placeholder="Add an item">
    <label>
      <input type="checkbox" v-model="newItemHighPriority">
      High Priority
    </label>
    <button :disabled="newItem.trim() === ''" class="btn btn-primary">Save item</button>
  </form>
  <ul>
    <li 
      v-for="{id, label, purchased, highPriority} in reversedItems" 
      @click="togglePurchased(id)"
      :class="{strikeout: purchased, priority: highPriority}"
      :key="id"
    >
      {{ label }}
    </li>
  </ul>
  <p v-if="items.length < 1">No items in the list.</p>
</template>