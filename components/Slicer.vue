<template>
  <button @click="toggleColumn(title)">
    {{ title }}
  </button>
</template>

<script>
export default {
  props: {
    title: {
      required: true,
      type: String
    }
  },
  methods: {
    toggleColumn (title) {
      this.$emit('toggleColumn', title)
    }
  }
}
</script>
