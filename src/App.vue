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
      boxRef.value.classList.remove("stop-2")
      boxRef.value.classList.add('start')
      isRolling.value = true
    }
    const handleStop = () => {
      boxRef.value?.classList.remove("start")
      boxRef.value?.classList.add('stopping')
      // boxRef.value?.classList.add('stop-2')

      isRolling.value = false

      setTimeout(() => {
        boxRef.value?.classList.replace("stopping", "stop")
        boxRef.value?.classList.add("stop-2")
      }, 1000* 1)
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
@keyframes stop-1 {
  0% {
    transform: translateY(calc(-100% + 5rem));
  }

  100% {
    transform: translateY(0%);
  }
}
@keyframes stop-2 {
  0% {
    transform: translateY(calc(-100% + 5rem));
  }

  100% {
    transform: translateY(calc(-5rem));
  }
}
@keyframes stop-3 {
  0% {
    transform: translateY(calc(-100% + 5rem));
  }

  100% {
    transform: translateY(calc(-5rem * 2));
  }
}
@keyframes stop-4 {
  0% {
    transform: translateY(calc(-100% + 5rem));
  }

  100% {
    transform: translateY(calc(-5rem * 3));
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
      animation-duration: 2.5s;
      animation-iteration-count: 1;
      animation-timing-function: ease-out;
      animation-fill-mode: forwards;
    }
    &.stop-1 {
      animation-name: stop-1;
    }
    &.stop-2 {
      animation-name: stop-2;
    }
    &.stop-3 {
      animation-name: stop-3;
    }
    &.stop-4 {
      animation-name: stop-4;
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
