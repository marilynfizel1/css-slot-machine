<template>
  <div class="container">
    <!-- <div class="no">NO: {{count}}</div> -->
    <div ref="wrapperRef" class="box-wrapper">
      <div v-for="box in 6" :key="box" class="box">
        <div>0</div>
        <div v-for="n in 9" :key="n">{{n}}</div>
        <div class="">0</div>
      </div>
    </div>
  </div>
  <div class="footer">
    <button v-if="isRolling" class="btn" @click="handleStop">
      STOP
    </button>
    <button v-else class="btn" @click="handleStart" :disabled="count >= numList.length">
    START
    </button>
  </div>
  <!-- <div class="num">{{count}} 次</div> -->
  <div class="num">
    <div v-for="(n,i) in numList" :key="i" :class="{active: count === i+1}">
      ({{i+1}}) {{n}}
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent,onMounted,ref,watchEffect } from "@vue/runtime-core";

export default defineComponent({
  setup(props) {
    const isRolling = ref(false)
    const wrapperRef = ref<HTMLDivElement>()
    const getRandomNum = () => Math.floor(Math.random() * Math.pow(10, 6)).toString().padEnd(6, "0")
    const count = ref(0)
    // const num = ref<string>(getRandomNum())
    const numList = ref<string[]>(Array(6).fill("").map(t=> getRandomNum()))


    onMounted(() => {
      wrapperRef.value?.querySelectorAll(".box").forEach((box, i) => {
        const _box = box as HTMLDivElement
        _box.style.transform = `translateY(calc(-5rem * (${getRandomNum().charAt(i)})))`
      })
    })

    const handleStart = () => {
      if(!wrapperRef.value) return
      ++count.value
      wrapperRef.value?.querySelectorAll(".box").forEach(box => {
        const _box = box as HTMLDivElement
        _box.classList.remove("stop")
        _box.classList.add('start')
      })
      isRolling.value = true
    }
    const handleStop = () => {
       wrapperRef.value?.querySelectorAll(".box").forEach((box, i) => {
        const _box = box as HTMLDivElement
        _box.classList.remove("start")
        _box.classList.add('stopping')
        _box.style.setProperty("--target-num", numList.value[count.value -1].charAt(i))
         setTimeout(() => {
          _box.classList.replace("stopping", "stop")
        }, 500 * (i + 1))
      })
      isRolling.value = false


    }
    return {
      isRolling,
      handleStart,
      handleStop,
      wrapperRef,
      numList,
      count
    }
  }
})
</script>
<style lang="scss">
:root {
  font-size: 16px;
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
  font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
}



.container {
  margin: 0 auto;
  width: fit-content;
  .no {
    font-family: 'Courier New', Courier, monospace;
    font-size: 3rem;
    font-weight: bold;
  }
  .box-wrapper {
    height: 5rem;
    background-color: lightgray;
    overflow: hidden;
    width:fit-content;
    margin: 0 auto;
    margin-bottom: 1.5rem;
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
        border-right: 1px solid rgba(#fff,0.2);
        /* border-bottom: 1px solid rgba(#fff,0.2) */
      }
    }
  }
}

.num {
  font-size: 1.5rem;
  font-weight: bold;
  color:gray;
  width: fit-content;
  margin-top: 1rem;
  > div {
    padding: 0.7rem 1rem;
    border-bottom: 1px solid #ccc;
    &.active {
      color:chocolate;
    }
  }
}

.footer {
  text-align: center;

}

.btn {
  background-color: chocolate;
  border: none;
  padding: 0.7em 1.5em;
  color: #fff;
  font-size: 1.2em;
  cursor: pointer;
  border-radius: 0.3em;
  font-family:Verdana, Geneva, Tahoma, sans-serif;
  &:hover {
    background-color:darken(chocolate, 5%);
  }
  &:disabled {
    cursor: not-allowed;
    background-color: gray;
    color: #555;
  }
}


</style>
