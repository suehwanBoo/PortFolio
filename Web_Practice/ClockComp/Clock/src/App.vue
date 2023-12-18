<template>
  <div class="container">
      <div class="clock">
          <div class="point"></div>
          
          <div class="circle">
              <div class="move" style="top: 100px;">
                  <h2 style="font-weight: bold; font-family:'Playfair Display', serif; color: rgba(63, 62, 62, 0.87);" >B S H</h2>
                  <div><p style="font-weight: bold; margin-top: 60px; z-index: 999;">{{ hours }} : {{ minutes }} : {{ seconds }}</p></div>
              </div>
          
              <div style="width: 550px;  margin: 0px; border: 1px; position: absolute;" id="seconds">
                  <div style=" width: 275px; border: 1px solid  red;"></div>
                  <div style=" width: 275px; border: 1px solid  white;"></div>
              </div>
              <div style="width: 450px;  margin: 0px; border: 1px;position: absolute; " id="minutes">
                  <div style=" width: 225px; border: 1px solid  black;"></div>
                  <div style=" width: 225px; border: 1px solid  white;"></div>
              </div>
              <div style="width: 380px;  margin: 0px; border:  2px;position: absolute; " id="hours">
                  <div style=" width: 190px; border: 2px solid  black;"></div>
                  <div style=" width: 190px; border: 2px solid  white;"></div>
              </div>

          </div>
      </div>
      <div>
          
      </div>
  </div>
</template>

<script setup>
import {ref, onMounted, watch} from 'vue'

const hours = ref()
const minutes = ref()
const seconds = ref()

onMounted(()=>{
  setInterval(()=>{
      let date = new Date();
      hours.value = date.getHours(); 
      minutes.value = date.getMinutes();
      seconds.value = date.getSeconds();
  },100)

  // 눈금 만들기
  for(let i = 0 ;i<180; i+=6){
      const clock = document.querySelector('.clock')
      const line = document.createElement('div')
      line.style.width = '600px'
      line.style.backgroundColor = 'black'
      line.style.position = 'absolute'
      line.style.transform = `rotate(${i}deg)`
      clock.appendChild(line)
      if(i%5==0){
          line.style.height = '3px'
      }
      else{
          line.style.height = '1px'
      }
  }

  watch(seconds,(newValue,oldValue)=>{
      const secDiv = document.querySelector('#seconds')
      secDiv.style.transform = `rotate(${newValue*6+90}deg)`
})
  watch(minutes,(newValue,oldValue)=>{
      const minDiv = document.querySelector('#minutes')
      minDiv.style.transform = `rotate(${newValue*6+90}deg)`
})
watch(hours,(newValue,oldValue)=>{
      const hourDiv = document.querySelector('#hours')
      hourDiv.style.transform = `rotate(${newValue*30+90}deg)`
})
})



</script>

<style scoped>
.container{
  display: flex;
  justify-content: center;
  align-items: center;
}
.move{
  position: absolute;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.clock {
width: 600px;
height: 600px;
border: 1px solid black;
border-radius: 50%; 
  /* 원형태를 만듦 */
display: flex;
justify-content: center;
align-items: center;
position: relative;
}
.point{
  width:15px;
  height:15px;
  border-radius: 50%;
  border:1px solid black;
  background-color: white;
  z-index: 30;
}

.circle{
  width: 585px;
  height: 585px;
  border: 1px solid black;
  border-radius: 50%;
  z-index: 10;
  position: absolute;
  background-color: white;
  display: flex;
  justify-content: center;
  align-items: center;
}

</style>