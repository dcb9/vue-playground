<template>
  <div class="container">
    <h2>{{ title }}</h2>
    <add-item v-on:addItem="addItem"></add-item>
    <items :items="items"></items>
    <change-title v-model="title"></change-title>
  </div>
</template>

<script>

import AddItem from './components/AddItem'
import Items from './components/Items'
import ChangeTitle from './components/ChangeTitle'

const data = {
  title: "My Shopping List",
  items: [
    {text: "Orange", checked: true},
    {text: "Grape", checked: false},
    {text: "Cucumber", checked: true},
  ],
}
window.data = data

export default {
  name: 'app',
  data: () => data,
  components: {
    AddItem,
    Items,
    ChangeTitle,
  },
  methods: {
    addItem (item) {
      this.items.push(item)
    },
  },
}

</script>

<style>
.add-item {
  margin-bottom: 10px;
}

.hidden {
  display: none;
}

.removed > div > label {
  text-decoration: line-through
}
</style>
