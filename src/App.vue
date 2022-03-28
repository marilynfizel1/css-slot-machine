<template>
  <div class="container">
    <div class="box" :class="{start: isRolling, stop: isStoping}">
      <div v-for="n in 4" :key="n">{{n}}</div>
      <div class="">1</div>
    </div>
  </div>
  <button v-if="isRolling" class="" @click="handleStop">
    STOP
  </button>
  <button v-else class="" @click="handleStart">
   START
  </button>
</template>
<script lang="ts">
import { defineComponent,ref } from "@vue/runtime-core";

export default defineComponent({
  setup(props) {
    const isRolling = ref(false)
    const isStoping = ref(false)
    const handleStart = () => {
      isRolling.value = true
      isStoping.value = false
    }
    const handleStop = () => {
      isRolling.value = false
      isStoping.value = true
    }
    return {
      isRolling,
      isStoping,
      handleStart,
      handleStop,
    }
  }
})
</script>
<style lang="scss">
@keyframes rolling {
  0% {
    transform: translateY(calc(-100% + 80px));
  }
  100% {
    transform: translateY(0%);
  }
}
@keyframes stoping {
  0% {
    transform: translateY(calc(-100% + 80px));
  }

  100% {
    transform: translateY(calc(0% - 80px));
  }
}
body {
  padding: 100px;
}

.container {
  height: 5rem;
  background-color: lightgray;
  overflow: hidden;
  .box {
    /* animation: rolling 2s ease-in; */
    transform-origin: bottom;
    text-align: center;
    background-color: darkgreen;
    color: yellowgreen;
    width: min-content;
    position: relative;
    &.start {
      animation-name: rolling;
      animation-duration: 0.5s;
      animation-iteration-count: infinite;
      animation-timing-function: linear;
      animation-fill-mode: forwards;
    }
    &.stop {
      animation-name: stoping;
      animation-duration: 5s;
      animation-iteration-count: 1;
      animation-timing-function: ease-out;
      animation-fill-mode: forwards;
    }
    > div {
      width: 5rem;
      height: 5rem;
      display: grid;
      font-size: 2rem;
      place-items: center;
      border-bottom: 1px solid rgba(#fff,0.2)
    }
  }
}
</style>
