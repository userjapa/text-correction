<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>Text Correction</h1>
    <h2>Game</h2>
    <div class="game-tc">
      <div class="game-tc__media">
        <audio ref="audio"
               :src="question.audio"
               @playing="playing = true"
               @pause="playing = false"
               @loadeddata="duration = $event.target.duration"
               @timeupdate="time = $event.target.currentTime"/>
        <div class="game-tc__media__player">
          <span @click="playing?$refs['audio'].pause():$refs['audio'].play()">{{ playing?'Pause':'Play' }}</span>
          <input type="range"
                 step="0.00001"
                 v-model="time"
                 :max="duration"
                 @input="$refs['audio'].currentTime = time">
        </div>
      </div>
      <div class="game-tc__answer">
        <input type="text"
               name="answer"
               v-model="question.answer"
               :class="{
                 right: (question.answered && question.hit),
                 wrong: (question.answered && !question.hit)
               }">
      </div>
      <div class="game-tc__options" v-if="!ended">
        <button type="button" name="confirm" @click="confirm(question)" :disabled="!question.answer" v-if="!question.answered">Confirm</button>
        <button type="button" name="next" @click="setCurrent(questions)" v-else>Next</button>
      </div>
      <div class="game-tc__options" v-else>
        <button type="button" name="finish">Finish</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      questions: [{
        audio: 'https://vignette.wikia.nocookie.net/leagueoflegends/images/e/ef/Kayn.attack22.ogg/revision/latest?cb=20170918212301',
        question: 'Found, you!',
        answer: '',
        answered: false,
        hit: false
      }],
      question: {},
      ended: false,
      index: 0,
      playing: false,
      time: 0,
      duration: 0
    }
  },
  methods: {
    setCurrent (questions) {
      let index = 0
      let qst = {}
      for (const ind in questions) {
        index = ind
        if (!questions[ind].answered) {
          qst = questions[ind]
          break
        } else if (ind == (questions.length - 1)) {
          qst = questions[ind]
          this.ended = true
        }
      }
      this.$set(this, 'question', qst)
      this.$set(this, 'index', index)
    },
    confirm (question) {
      const text = question.question.replace(/[,.!?]/g, '').toLowerCase()
      const answer = question.answer.replace(/[,.!?]/g, '').toLowerCase()
      this.$set(question, 'answered', true)
      this.$set(question, 'hit', text == answer)
    }
  },
  mounted () {
    this.setCurrent(this.questions)
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
  div:not(:last-child) {
    margin-bottom: 15px;
  }
  &__media {
    display: flex;
    justify-content: center;
    &__player {
      width: 300px;
      height: 39px;
      border: 2px solid gray;
      border-radius: 25px;
      display: flex;
      span {
        cursor: pointer;
        display: inline-block;
        border: 2px solid gray;
        border-radius: 25px;
        padding: 5px;
        margin: 5px;
        flex-basis: calc(20% - 20px);
      }
      input {
        height: 100%;
        margin: 0;
        margin-right: 15px;
        flex-basis: calc(80% - 15px);
      }
    }
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
      &.right {
        background-color: green;
      }
      &.wrong {
        background-color: red;
      }
    }
  }
  &__options {
    button {
      font-size: 16px;
      width: 300px;
      cursor: pointer;
      padding: 10px 30px;
      border: none;
      border-radius: 25px;
    }
  }
}
</style>
