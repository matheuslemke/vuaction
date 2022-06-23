<script lang="ts">
import { setTimeout } from 'timers'
import Vue from 'vue'
import MainHeader from '../components/MainHeader.vue'
import PrimaryButton from '../components/PrimaryButton.vue'

const MAX_SECONDS = 10
const MIN_SECONDS = 1
const ATTEMPTS = 5
let times: number[] = []

function nextStage(stage: number, guessClicked: boolean) {
  console.log('currentStage', stage)
  if (stage >= ATTEMPTS || guessClicked) {
    return -1
  }
  setTimeout(() => nextStage(++stage, false), times[stage] * 1000)
  return 0
}

export default Vue.extend({
  name: 'IndexPage',
  components: { MainHeader, PrimaryButton },
  data() {
    return {
      testStarted: false,
      guessClicked: false,
      shouldShowResults: false,
    }
  },
  methods: {
    startTest() {
      this.testStarted = true

      times = Array(ATTEMPTS)
        .fill(null)
        .map(
          () => Math.random() * (MAX_SECONDS - MIN_SECONDS + 1) + MIN_SECONDS
        )

      nextStage(0, this.guessClicked)
    },
    retry() {
      this.testStarted = false
      this.guessClicked = false
    },
  },
})
</script>

<template>
  <div class="min-h-screen flex flex-col">
    <MainHeader />
    <div
      v-if="!testStarted"
      class="bg-gray-300 flex text-center items-center place-content-center centered"
    >
      <PrimaryButton title="Start test" @clicked="startTest" />
    </div>

    <div
      v-if="testStarted && !guessClicked"
      class="cursor-pointer bg-red-500 flex text-center text-3xl text-gray-100 tracking-widest leading-loose items-center place-content-center centered"
      @click="guessClicked = true"
    >
      Wait for green...
    </div>

    <div
      v-if="testStarted && guessClicked"
      class="cursor-pointer bg-yellow-400 flex flex-col text-center items-center place-content-center centered"
      @click="retry"
    >
      <div class="text-3xl text-gray-70 tracking-widest leading-loose">
        Don't try to guess
      </div>
      <div class="text-xl text-gray-70">Click to retry</div>
    </div>
    <div v-if="shouldShowResults">
      Results here
      <button>Next try</button>
      <button>Restart</button>
    </div>
  </div>
</template>

<style>
.centered {
  min-height: calc(100vh - 120px);
}
</style>
