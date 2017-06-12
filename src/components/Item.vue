<template>
  <li class="list-group-item" v-bind:class="{ 'removed': item.checked }">
      <div class="checkbox">
        <label>
          <input type="checkbox" v-model="item.checked">
          {{ item.text }}
        </label>
      </div>
  </li>
</template>
<script>
export default {
  name: "item",
  props: ["item"],
}
</script>
