<template>
  <div class="footer">
    <hr>
    <em>Change the title of your shopping list here</em>
    <input :value="value" class="form-control" v-on:input="onInput">
  </div>
</template>

<script>
export default {
  name: "change-title",
  props: ["value"],
  methods: {
    onInput (event) {
      this.$emit('input', event.target.value)
    },
  }
}
</script>
