<template>
  <div id="app">
    <QuizHeader
      :numCorrect="numCorrect"
      :numTotal="numTotal"
      :isQuizOver="isQuizOver"
    />

    <b-container 
      class="bv-example-row"
      v-if="!isQuizOver"
    >
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox 
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
            :isQuizOver="isQuizOver"
          />
        </b-col>
      </b-row>
    </b-container>

    <b-container 
      class="bv-example-row"
      v-if="isQuizOver"
    >
      <QuizResult 
        :numCorrect="numCorrect"
        :numTotal="numTotal"
      />
    </b-container>

   
  </div>
</template>

<script>
import QuizHeader from './components/QuizHeader.vue'
import QuestionBox from './components/QuestionBox.vue'
import QuizResult from './components/QuizResult.vue'

export default {
  name: 'App',
  components: {
    QuizHeader,
    QuestionBox,
    QuizResult
  },
  data() { //need a function to return data object when making single page apps, not only having the data object
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      isQuizOver: false,
    }
  },
  methods: {
    next() {
      this.index++
      this.isQuizOver = this.index === this.questions.length
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
      method: 'get'
    })
    .then((response) => {
      return response.json();
    })
    .then((jsonData) => {
      this.questions = jsonData.results;
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
