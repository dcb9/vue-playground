<template>
  <ul class="list-group">
    <item v-for="item in items" :item="item"></item>
  </ul>
</template>
<script>
import Item from './Item.vue'

export default {
  name: "items",
  components: {
    Item,
  },
  props: ["items"],
}
</script>
