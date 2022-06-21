<template>
  <div>
    <h1>Vueaction</h1>
    <div v-if="!testStarted">
      <h4>Test your reaction time</h4>
      <button @click="testStarted = true">Start test</button>
    </div>
    <div v-if="testStarted">Wait for green</div>
    <div>Click it!!!</div>
    <div>
      Results here
      <button>Next try</button>
      <button>Restart</button>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'IndexPage',
  data() {
    return {
      testStarted: false,
    }
  },
})
</script>
