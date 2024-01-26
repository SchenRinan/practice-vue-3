<template>
  <div class="p-5 text-center">
    <h1>Reaction Timer</h1>
    <p>Click to start, wait till its red, click as fast as you can</p>
    <p>{{ score }}<span v-if="score > 0">ms</span> <span v-if="result">{{ result }}</span></p>
    <button class="btn btn-primary p-5" type="button" @click="handleClick('start')" v-if="showStart">Start</button>
    <button class="btn btn-info p-5" type="button" @click="handleClick('wait')" v-if="showWait">Wait</button>
    <button class="btn btn-danger p-5" type="button" @click="handleClick('win')" v-if="showClick">Click!</button>
    <ul class="list-group pt-4 col-2 mx-auto">
      <li class="list-group-item">Top Speed</li>
      <li class="list-group-item" v-for="speed in tSpeed" :key="speed">{{ speed }}ms</li>
    </ul>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
name: "RTimer",
setup(){
  const showStart = ref(true)
  const showWait = ref(false)
  const showClick = ref(false)
  let delay = 0
  let timer = 0
  const score = ref(0)
  const result = ref('')
  const tSpeed = ref([])

  const handleClick = (value) =>{
    if (value === 'start'){
      score.value = 0
      showStart.value = !showStart.value
      showWait.value = !showWait.value
      result.value = ''
      delay = setTimeout(() => {
        showStart.value = false
        showWait.value = false
        showClick.value = true
        timer = setInterval(() => {score.value += 10}, 10)
      }, 1000+2000*Math.random())
      //create a delay delay to pop up show click that's from 2-5 secs
    }
    if (value === 'wait'){
      score.value = 'You lose! Did not wait for red button'
      showStart.value = !showStart.value
      showWait.value = !showWait.value
      clearTimeout(delay)
      delay = 0
    }
    if (value === 'win'){
      clearInterval(timer)
      timer = 0
      showStart.value = true
      showWait.value = false
      showClick.value = false
      tSpeed.value.push(score.value)
      tSpeed.value.sort()
      if(tSpeed.value.length > 5){tSpeed.value.pop()}
      if(score.value <200 && score.value > 0){
        result.value = 'Super fast reflexes!'
      }
      else if(score.value <400){ result.value = 'Average'}
      else if(score.value >400) { result.value = 'Slow'}
    }
  }

  return {showStart, showWait, showClick, score, result, tSpeed, handleClick}
}
}
</script>

<style>

</style>