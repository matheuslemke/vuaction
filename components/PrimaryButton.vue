<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'PrimaryButton',
  props: {
    title: {
      type: String,
      default: '',
    },
  },
  methods: {
    handleClick() {
      this.$emit('clicked')
    },
  },
})
</script>

<template>
  <button
    class="bg-green-700 hover:bg-green-900 text-gray-100 py-2 px-4 rounded shadow"
    @click="handleClick"
  >
    {{ title }}
  </button>
</template>
