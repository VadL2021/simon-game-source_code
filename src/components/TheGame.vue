<template>
  <div>
    <div class="gameTablet">
      <div @click="gameProcess(0); playSound(0)" class="yellow"
          :class="{active: (activeButton == 0)}"> </div>
      <div @click="gameProcess(1); playSound(1)" class="blue" 
          :class="{active: (activeButton == 1)}"> </div>
      <div @click="gameProcess(2); playSound(2)" class="green"
          :class="{active: (activeButton == 2)}"> </div>
      <div @click="gameProcess(3); playSound(3)" class="red"
          :class="{active: (activeButton == 3)}"> </div>
    </div>
    <div class="controlTablet">
      <p>Выберите сложность</p>
      <input 
        name="easyCheckbox"
        type="checkbox" 
        value="1.5"
        v-model="difficultLvl"
        :class="{
          hidden: (difficultLvl[0] != '1.5' && difficultLvl.length != 0)
          }"
        >
        <label 
          :class="{hidden: (difficultLvl[0] != '1.5' && difficultLvl.length != 0)}"
          for="easyCheckbox"
          > Легкий уровень</label>
      <br>
      <input 
        name="mediumCheckbox"
        type="checkbox" 
        value="1" 
        v-model="difficultLvl"
        :class="{hidden: (difficultLvl[0] != '1' && difficultLvl.length != 0)}"
        >
        <label 
          for="mediumCheckbox"
          :class="{hidden: (difficultLvl[0] != '1' && difficultLvl.length != 0)}"
          > Средний уровень</label>
      <br>
      <input 
        name="hardCheckbox"
        type="checkbox" 
        value="0.4"  
        v-model="difficultLvl"
        :class="{hidden: (difficultLvl[0] != '0.4' && difficultLvl.length != 0)}"
        >
        <label 
          for="hardCheckbox"
          :class="{hidden: (difficultLvl[0] != '0.4' && difficultLvl.length != 0)}"
          > Сложный уровень</label>
      <br>
      <button 
        v-if="difficultLvl.length != 0"
        @click="gameStart()"
        >Старт</button>
      <p>---------------</p>
      <p> Уровень: {{ gameArray.length }}</p>
    </div>
  </div>
</template>

<script>
import do_mp3 from '../assets/do.mp3'
import re_mp3 from '../assets/re.mp3'
import mi_mp3 from '../assets/mi.mp3'
import sol_mp3 from '../assets/sol.mp3'

export default {
  name: 'TheGame',
  props: {
    msg: String
  },
  data(){
    return {
      difficultLvl:[],
      gameArray:[],
      playerArray:[],
      activeButton:null
    }
  },
  methods:{
    gameStart(){
      this.gameArray = []
      this.playerArray = []
      this.gameArray.push(this.getRandomInt(4))
      this.playSound(this.gameArray[0])
      this.activeButton = this.gameArray[0]
      setTimeout(()=>{this.activeButton = null}, 300)
    },
    gameProcess(chosenButton){
      if(this.gameArray.length != this.playerArray.length){
        this.playerArray.push(chosenButton)
        if(this.gameArray[this.playerArray.length-1] != this.playerArray[this.playerArray.length-1]){
          this.gameArray = []
          this.playerArray = []
          alert('Игра окончена!')
        }
        if(this.gameArray.length == this.playerArray.length && this.gameArray.length != []){
          this.gameArray.push(this.getRandomInt(4))
          this.gameArray.forEach( (el, index) => setTimeout(() => {
            this.playSound(el); 
            this.activeButton = el
            setTimeout(()=>{this.activeButton = null}, (index*this.difficultLvl[0])*1000+600)
          }, (index*this.difficultLvl[0])*1000+500) )
          this.activeButton = null
          this.playerArray = []
        } 
      }
    },
    playSound(sound){
      let audio = new Audio
      switch(sound){
        case 0:
          audio = new Audio(do_mp3)
          break 
        case 1:
          audio = new Audio(mi_mp3)
          break 
        case 2:
          audio = new Audio(re_mp3)
          break
        case 3:
          audio = new Audio(sol_mp3)
          break     
      }
      audio.play()
    },
    getRandomInt(max) {
      return Math.floor(Math.random() * max);
    }
  }
}
</script>
