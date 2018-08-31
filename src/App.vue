<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>Text Correction</h1>
    <h2>Game</h2>
    <div class="game-tc">
      <div class="game-tc__media">
        <audio :src="question.audio" controls/>
      </div>
      <div class="game-tc__answer">
        <input type="text" name="" v-model="question.answer">
      </div>
      <div class="game-tc__options">
        <button type="button" name="confirm" @click="confirm(question)" :disabled="!question.answer">Confirm</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      question: {
        audio: 'https://vignette.wikia.nocookie.net/leagueoflegends/images/e/ef/Kayn.attack22.ogg/revision/latest?cb=20170918212301',
        question: 'Found you',
        answer: '',
        answered: false,
        hit: false
      }
    }
  },
  methods: {
    confirm (question) {
      const text = question.question
      const answer = question.answer
      this.$set(question, 'answered', true)
      this.$set(question, 'hit', text == answer)
      console.log(question)
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

.game-tc {
  &, & div {
    border: 1px solid black;
  }
  div:not(:last-child) {
    margin-bottom: 15px;
  }
  &__answer {
    input {
      background-color: lighten(yellow, 20);
      border-radius: 25px;
      border: none;
      padding: 11px 15px;
      font-size: 16px;
      width: 270px;
      text-align: center;
    }
  }
  &__options {
    button {
      cursor: pointer;
      padding: 10px 30px;
      border: none;
      border-radius: 25px;
    }
  }
}
</style>
