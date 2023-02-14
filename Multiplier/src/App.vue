<template>
  <main class="template">

    <div id="backgroundAnswer" class="backgroundAnswer" @click="backToGame"></div>
    
    <div class="main">

      <section class="rightWrongCount" v-if="game">
        <div class="rightCount">Верные ответы: {{rightCount}}</div>
        <div class="wrongCount">Ошибки: {{wrongCount}}</div>
      </section>

      <div class="clue" v-if="game">Вычислите значение и впишите ответ в строку:</div>

      <section class="example" v-if="game">
        <div class="number">{{this.firstNumber}}</div>
        <div class="sign">X</div>
        <div class="number">{{this.secondNumber}}</div>
      </section>

      <h1 class="title" v-else>Проверьте свои знания!</h1>

      <section class="result" v-if="game">
        <button class="result__button" @click="returnMenu">Вернуться в меню</button>
        <input type="number" class="answer" placeholder="Введите ответ" v-model="userAnswer"/>
        <button class="result__button" @click="checkResult">Проверить</button>
      </section>

      <button class="startButton" @click="startGame" v-else>Начать!</button>
    </div>
  </main>
</template>

<script>

import { random } from 'mathjs'

export default {
  data(){
    return{
      game: false,
      firstNumber: 0,
      secondNumber: 0,
      answer: 0,
      userAnswer: "",
      result: false,
      rightCount: 0,
      wrongCount: 0
    }
  },
  
  methods: {
    startGame() {
      this.game = true
      this.generationNumbers()
    },

    returnMenu(){
      this.rightCount = this.wrongCount = this.userAnswer = this.game = 0
    },

    generationNumbers(){
      this.firstNumber = ~~random(1, 10)
      this.secondNumber = ~~random(1, 10)
      this.answer = this.firstNumber * this.secondNumber
    },

    checkResult(){
      this.result = this.answer === this.userAnswer
      this.result === true ? this.rightCount++ : this.wrongCount++
      console.log(this.rightCount, this.wrongCount)
      this.userAnswer = ""
      this.generationNumbers()
      this.backgroundColor()
    },

    backgroundColor(){
      let background = document.getElementById("backgroundAnswer")
      background.style.display = "block"
      background.style.backgroundColor = this.result === true ? "green" : "red"
      setTimeout(this.backToGame, 1000)
    },

    backToGame(){
      document.getElementById("backgroundAnswer").style.display = "none"
    }
    
  },
  
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  color: white;
  font-family:Arial, Helvetica, sans-serif;
}

.backgroundAnswer{
  display: none;
  position: fixed;
  opacity: 0.5;
  width: 100vw;
  height: 100vh;
  z-index: 100;
}

.template{
  background: url("./img/background.jpg") center/cover no-repeat;
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.main{
  width: 60vw;
  height: 60vh;
  background-color: rgb(53, 63, 63);
  border: 5px solid rgb(255, 255, 255);
  border-radius: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content:space-around;
  padding: 30px;
}

.title{
  font-size: 2vw;
  text-align: center;
}

button{
  background: rgb(92, 92, 92);
  border: 2px solid rgb(179, 179, 179);
  border-radius: 20px;
  padding: 10px 20px;
  cursor: pointer;
  transition: 0.5s;
}

button:hover{
  scale: 1.1;
  opacity: 0.8;
}

.startButton{
  font-size: 3vw;
}

.clue{
  font-size: 2vw;
}

.example{
  display: flex;
  gap: 20px;
  font-size: 100px;
}

.answer{
  width: 20vw;
  height: 20vh;
  color: rgb(78, 78, 78);
  border: 2px solid black;
  border-radius: 20px;
  font-size: 40px;
  text-align: center;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
input[type=number] {
  -moz-appearance:textfield;
}

.result{
  display: flex;
  gap:30px;
  align-items: center;
  justify-content: space-around;
}

.result__button{
  font-size: 2vw;
  width: 15vw;
  height: 10vw;
}

.rightWrongCount{
  font-size: 1.5vw;
  display: flex;
  justify-content: space-around;
  width: 100%;
}

.rightCount{
  color: greenyellow;
}

.wrongCount{
  color: red;
}

@media only screen and (max-width : 1000px) {
  .title{
    font-size: 5vw;
  }

  .startButton{
    font-size: 5vw;
    padding: 20px 30px;
  }

  .rightWrongCount{
    font-size: 4vw;
    flex-direction: column;
    align-items: center;
    gap: 10px;
  }

  .clue{
    font-size: 4vw;
    text-align: center;
  }

  .answer{
    width: 40vw;
    height: 10vh;
    font-size: 4vw;
  }

  .result{
    gap:10px;
    flex-direction: column-reverse;
  }

  .result__button{
    font-size: 3vw;
    width: 40vw;
    height: 10vw;
  }

  button:hover{
    scale: 1;
    opacity: 1;
  }
}
</style>
