<template>
  <div class="input-group add-item">
    <input
       class="form-control"
       type="text"
       v-model="newItem"
       @keyup.enter="addItem"
       >
       <span class="input-group-btn"><button @click="addItem" class="btn btn-secondary">Add!</button></span>
  </div>
</template>

<script>
export default {
  name: "add-item",
  data () {
    return {
      newItem: "",
    }
  },
  props: ["value"],
  methods: {
    addItem() {
      let text = this.newItem.trim()
      if (text) {
        const item = {
          text,
          checked: false,
        }

        this.$emit("addItem", item)
        this.newItem = ''
      }
    },
  },
}
</script>
