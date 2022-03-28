<template>
  <div class="container">
    <div ref="boxRef" class="box">
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
import { defineComponent,onMounted,ref,watchEffect } from "@vue/runtime-core";

export default defineComponent({
  setup(props) {
    const isRolling = ref(false)
    const boxRef = ref<HTMLDivElement>()

    const handleStart = () => {
      if(!boxRef.value) return
      boxRef.value.classList.remove("stop")
      boxRef.value.classList.add('start')
      isRolling.value = true
    }
    const handleStop = () => {
      boxRef.value?.classList.remove("start")
      boxRef.value?.classList.add('stopping')
      boxRef.value?.style.setProperty("--target-num", "1")

      isRolling.value = false

      setTimeout(() => {
        boxRef.value?.classList.replace("stopping", "stop")
      }, 1000 * 1)
    }
    return {
      isRolling,
      handleStart,
      handleStop,
      boxRef,
    }
  }
})
</script>
<style lang="scss">
:root {
  font-size: 30px;
  --target-num: 2;
}

@keyframes rolling {
  0% {
    transform: translateY(calc(-100% + 5rem));
  }
  100% {
    transform: translateY(0%);
  }
}
@keyframes stopping {
  0% {
    transform: translateY(calc(-100% + 5rem));
  }

  100% {
    transform: translateY(0%);
  }
}
@keyframes stop {
  0% {
    transform: translateY(calc(-100% + 5rem));
  }

  100% {
    transform: translateY(calc(-5rem * (var(--target-num) - 1)));
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
      /* animation-fill-mode: forwards; */
    }
    &.stopping {
      animation-name: stopping;
      animation-duration: 1s;
      animation-iteration-count: infinite;
      animation-timing-function: linear;
      /* animation-fill-mode: forwards; */
    }
    &.stop {
      animation-name: stop;
      animation-duration: calc(1s * (1 + (4 - var(--target-num))));
      animation-iteration-count: 1;
      animation-timing-function: linear;
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
