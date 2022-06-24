<script lang="ts">
import Vue from 'vue'
import MainHeader from '../components/MainHeader.vue'
import PrimaryButton from '../components/PrimaryButton.vue'

const MIN_WAIT_SECONDS = 1
const MAX_WAIT_SECONDS = 4

export default Vue.extend({
  name: 'IndexPage',
  components: { MainHeader, PrimaryButton },
  data() {
    return {
      testStarted: false,
      shouldShowGreen: false,
      guessClicked: false,
      shouldShowResults: false,
      startTime: 0,
      reactionTime: 0,
      waitIntervalId: 0,
    }
  },
  head: {
    title: 'Vueaction',
  },
  methods: {
    startTest() {
      this.testStarted = true
      this.shouldShowGreen = false
      this.guessClicked = false
      this.shouldShowResults = false
      const waitTime =
        (Math.random() * (MAX_WAIT_SECONDS - MIN_WAIT_SECONDS + 1) +
          MIN_WAIT_SECONDS) *
        1000

      this.waitIntervalId = setInterval(() => {
        this.shouldShowGreen = true
        this.startTime = Date.now()
        clearInterval(this.waitIntervalId)
      }, waitTime)
    },
    guessed() {
      clearInterval(this.waitIntervalId)
      this.testStarted = true
      this.shouldShowGreen = false
      this.guessClicked = true
      this.shouldShowResults = false
    },
    reacted() {
      clearInterval(this.waitIntervalId)
      this.reactionTime = Date.now() - this.startTime
      this.shouldShowGreen = false
      this.testStarted = false
      this.shouldShowResults = true
    },
  },
})
</script>

<template>
  <div class="min-h-screen flex flex-col">
    <MainHeader />

    <div
      v-show="!testStarted && !shouldShowResults"
      class="bg-gray-300 flex text-center items-center place-content-center centered"
    >
      <PrimaryButton title="Start test" @clicked="startTest" />
    </div>

    <div
      v-show="testStarted && !guessClicked && !shouldShowGreen"
      class="cursor-pointer bg-red-500 flex text-center text-3xl text-gray-100 tracking-widest leading-loose items-center place-content-center centered"
      @click="guessed"
    >
      Wait for green...
    </div>

    <div
      v-show="shouldShowGreen && !guessClicked"
      class="cursor-pointer bg-green-400 flex flex-col text-center items-center place-content-center centered"
      @click="reacted"
    >
      <div class="text-3xl text-gray-70 tracking-widest leading-loose">
        Click it!!!
      </div>
    </div>

    <div
      v-show="testStarted && guessClicked"
      class="cursor-pointer bg-yellow-400 flex flex-col text-center items-center place-content-center centered"
      @click="startTest"
    >
      <div class="text-3xl text-gray-70 tracking-widest leading-loose">
        Don't try to guess
      </div>
      <div class="text-xl text-gray-70">Click to retry</div>
    </div>

    <div
      v-show="shouldShowResults"
      class="bg-gray-300 flex flex-col text-center items-center place-content-center centered"
    >
      <div class="text-3xl text-gray-70 tracking-widest leading-loose">
        Your reaction time is
        <span class="text-5xl">{{ reactionTime }}</span> milliseconds
      </div>
      <PrimaryButton title="Try again" @clicked="startTest"></PrimaryButton>
    </div>
  </div>
</template>

<style>
.centered {
  min-height: calc(100vh - 120px);
}
</style>
