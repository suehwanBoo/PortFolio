<template>
  <div>

    <br>
    <button
    @click="showCalendar"
      class="calButton"
    ><img src="@/assets/calendar.png" alt="" style="width: 20px;">
  </button>
    <div class="calendar">
      <div style="display: flex; justify-content: center; align-items: center; margin-top: 10px;">
        <button class="leftButton" @click="prev()"></button>
        <span style="color: white; margin-left: 10px; margin-right: 10px;">{{ todayYear }}, {{ todayMonth }}</span>
        <button class="rightButton" @click="next()"></button>
      </div>
      <table>
        <tr>
          <th v-for="index,key in weeks" :key="key" class="weeks">
          {{ index }}
          </th>
        </tr>
        <tbody>
          <tr v-for="week,key in MonthInfo" :key="key">
            <td v-for="day,key in week" :key="key" id="day" style="width: 31px; height: 24px;">
              {{ day }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>

  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';


function showCalendar(){
    const calendarDiv = document.querySelector('.calendar')
    console.log(calendarDiv.style.display)
    if(calendarDiv.style.display=='none'){
      calendarDiv.style.display ='block'
    }
    else{
      calendarDiv.style.display='none'
    }
  }
const px = ref('0px')
const calMouse = ref({'border': px.value})
const date = new Date();
const now = date.getDate()

const todayMonth = ref(date.getMonth().valueOf()+1)
console.log(date.getMonth())
const todayYear = ref(date.getFullYear().valueOf())
console.log(date.getFullYear())
const weeks = ref(['SUN','MON','TUE','WED','THU','FRI','SAT'])
const nowMonthStart = ref(new Date(todayYear.value,todayMonth.value-1,1))
const nowMonthEnd = ref(new Date(todayYear.value,todayMonth.value,0))
const nowMonth = ref([])
const MonthInfo = ref([])
const showMonth = function(){
    nowMonth.value = []
    let j = 1
    for(let i=0; i<nowMonthEnd.value.getDate()+nowMonthStart.value.getDay();i++){
      if(i<nowMonthStart.value.getDay()){
        nowMonth.value.push('')
      }
      else{
        nowMonth.value.push(j)
        j++;
      }
  }
  MonthInfo.value = []
  for(let i=0; i<nowMonth.value.length;i+=7){
      const temp = nowMonth.value.slice(i,i+7)
      while(temp.length<7){
        temp.push('　')
      }
      MonthInfo.value.push(temp)   
    }
    if(MonthInfo.value.length<6){
      MonthInfo.value.push(['　'])
    }
  
}
const prev = function(){
  todayMonth.value -= 1
  if(todayMonth.value == 0){
    todayMonth.value = 12
    todayYear.value -=1
  }
  checkToday()
}
const next = function(){
  todayMonth.value += 1
  if(todayMonth.value == 13){
    todayMonth.value = 1
    todayYear.value += 1
  }
  checkToday()
}
const checkToday = function(){
  setTimeout(()=>{
    const days = document.querySelectorAll('#day')

  if(todayMonth.value==date.getMonth()+1 && todayYear.value == date.getFullYear()){
    for(let i=0; i<days.length;i++){
      if(+days[i].innerHTML===date.getDate())
      days[i].classList.toggle('changeDay')
    }
  }
  else{
    for(let i=0; i<days.length;i++){
      days[i].classList.remove('changeDay')
    }
  }
  },100)
 
  
}
onMounted(()=>{
  showMonth()
  checkToday()
  watch(todayMonth,(newValue,oldValue)=>{
    nowMonthStart.value = new Date(todayYear.value,todayMonth.value-1,1)
    nowMonthEnd.value = new Date(todayYear.value,todayMonth.value,0)
    showMonth()
    
  })})

</script>

<style scoped>
.calButton{
  border-radius: 15%;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  
}
.calendar{
  position: absolute;
  /* border: 1px solid black; */
  width: 300px;
  height: 240px;
  background-color: #333333;
  top: 410px;
  left: 607px;
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;

  border-radius: 15px;
}
.leftButton{
  border-top: 10px solid #333333;
  border-left: 10px solid #333333;
  border-right: 10px solid white;
  border-bottom: 10px solid #333333;
  content: "";
  border-radius: 4px;

  padding: 0;
}

.rightButton{
  border-top: 10px solid #333333;
  border-left: 10px solid white;
  border-right: 10px solid #333333;
  border-bottom: 10px solid #333333;
  border-radius: 4px;
  content: "";
  padding: 0;
}
.weeks{
  color: white;
  font-size: 13px;
  
}
.changeDay{
  color: yellow;
}

table{
  border-spacing: 5px;
  text-align: center;
  color: white;
  align-self: center;
  margin: auto;
}
tr td:last-child {
  color: rgb(57, 57, 215);
}

tr td:first-child {
  color: rgb(239, 75, 75);
}
tr td:hover{
  border: 1px solid white;
  padding: 0px;
}
</style>
<style>
body{
  height: 100vh;
}
</style>