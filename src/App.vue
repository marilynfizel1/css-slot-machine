<template>
  <div class="num">
    {{num}}
  </div>
  <div ref="containerRef" class="container">
    <div v-for="box in 6" :key="box" class="box">
      <div>0</div>
      <div v-for="n in 9" :key="n">{{n}}</div>
      <div class="">0</div>
    </div>
  </div>
  <div class="footer">
    <button v-if="isRolling" class="" @click="handleStop">
      STOP
    </button>
    <button v-else class="" @click="handleStart">
    START
    </button>
  </div>
</template>
<script lang="ts">
import { defineComponent,onMounted,ref,watchEffect } from "@vue/runtime-core";

export default defineComponent({
  setup(props) {
    const isRolling = ref(false)
    const containerRef = ref<HTMLDivElement>()
    const getRandomNum = () => Math.floor(Math.random() * Math.pow(10, 6)).toString()
    const num = ref(getRandomNum())
    watchEffect(() => {
    console.log(num.value)
    })

    onMounted(() => {
      containerRef.value?.querySelectorAll(".box").forEach((box, i) => {
        const _box = box as HTMLDivElement
        _box.style.transform = `translateY(calc(-5rem * (${num.value.charAt(i)})))`
      })
    })

    const handleStart = () => {
      if(!containerRef.value) return
      num.value = getRandomNum()
      containerRef.value?.querySelectorAll(".box").forEach(box => {
        const _box = box as HTMLDivElement
        _box.classList.remove("stop")
        _box.classList.add('start')
      })
      isRolling.value = true
    }
    const handleStop = () => {
       containerRef.value?.querySelectorAll(".box").forEach((box, i) => {
        const _box = box as HTMLDivElement
        _box.classList.remove("start")
        _box.classList.add('stopping')
        _box.style.setProperty("--target-num", num.value.charAt(i))
         setTimeout(() => {
          _box.classList.replace("stopping", "stop")
        }, 1000 * (i + 1))
      })
      isRolling.value = false


    }
    return {
      isRolling,
      handleStart,
      handleStop,
      containerRef,
      num
    }
  }
})
</script>
<style lang="scss">
:root {
  font-size: 12px;
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
/* @keyframes stopping {
  0% {
    transform: translateY(calc(-100% + 5rem));
  }

  100% {
    transform: translateY(0%);
  }
} */
@keyframes stop {
  0% {
    transform: translateY(calc(-100% + 5rem));
  }

  100% {
    transform: translateY(calc(-5rem * (var(--target-num))));
  }
}

body {
  padding: 100px 10px;
}

.num {
  font-size: 3rem;
  font-weight: bold;
  text-align: center;
  color:chocolate;
}

.footer {
  text-align: center;
}

.container {
  height: 5rem;
  background-color: lightgray;
  overflow: hidden;
  margin-bottom: 2rem;
  /* display: flex; */
  /* column-gap: 0.1rem; */
  .box {
    /* animation: rolling 2s ease-in; */
    display: inline-block;
    transform-origin: bottom;
    text-align: center;
    background-color: darkgreen;
    color: yellowgreen;
    width: min-content;
    position: relative;
    animation-name: rolling;
    animation-duration: 0s;
    animation-iteration-count: infinite;
    animation-timing-function: linear;
    &.start {
      animation-duration: 0.5s;
    }
    &.stopping {
      animation-duration: 1s;
    }
    &.stop {
      animation-name: stop;
      animation-duration: calc(0.5s * (1 + (9 - var(--target-num))));
      animation-iteration-count: 1;
      animation-fill-mode: forwards;
      animation-timing-function: ease-out;
    }

    > div {
      width: 5rem;
      height: 5rem;
      display: grid;
      font-size: 2rem;
      place-items: center;
      border-right: 1px solid rgba(#fff,0.2)
      /* border-bottom: 1px solid rgba(#fff,0.2) */
    }
  }
}
</style>
