<template>
  <div class="min-h-screen flex flex-col">
    <MainHeader />
    <div
      v-if="!testStarted"
      class="bg-gray-300 flex text-center items-center place-content-center centered"
    >
      <button
        class="bg-green-700 hover:bg-green-900 text-gray-100 py-2 px-4 rounded shadow"
        @click="testStarted = true"
      >
        Start test
      </button>
    </div>
    <div v-if="testStarted" class="cursor-pointer" @click="guessClick">
      <div
        class="bg-red-500 flex text-center text-3xl text-gray-100 tracking-widest leading-loose items-center place-content-center centered"
      >
        Wait for green
      </div>
    </div>
    <div v-if="shouldShowResults">
      Results here
      <button>Next try</button>
      <button>Restart</button>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import MainHeader from '../components/MainHeader.vue'

export default Vue.extend({
  name: 'IndexPage',
  components: { MainHeader },
  data() {
    return {
      testStarted: false,
      shouldShowResults: false,
    }
  },
  methods: {
    guessClick: () => {
      console.log('asdada')
    },
  },
})
</script>

<style>
.centered {
  min-height: calc(100vh - 120px);
}
</style>
