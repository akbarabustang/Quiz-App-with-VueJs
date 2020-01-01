<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        {{ htmlDecode(currentQuestion.question) }}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
          :disabled="answered === true"
        >
         {{ htmlDecode(answer) }}
        </b-list-group-item>
      </b-list-group>

      <b-button
       variant="primary" 
       @click="submitAnswer"
       :disabled="selectedIndex === null || answered"
       >Submit
      </b-button>

      <b-button 
        @click="next" 
        :disabled="numQuestion === totalQuestion"
        variant="success" 
        href="#"
      >
        Next
      </b-button>

    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
import he from 'he'

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
    numQuestion: Number,
    totalQuestion: Number
  },

  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswer: [],
      answered: false,
    }
  },

  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return this.shuffledAnswer
    }
  },

  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
      this.selectedIndex = null
      this.answered = false
      this.shuffleAnswers()
      }
    }
  },

  methods: {
    selectAnswer(index) {
      this.selectedIndex = index
    },

    submitAnswer() {
      let isCorret = false

      if (this.selectedIndex === this.correctIndex ) {
        isCorret = true
      }

      this.increment(isCorret)
      this.answered = true
    },

    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswer = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswer.indexOf(this.currentQuestion.correct_answer)
    },

    answerClass(index) {
      let answerClass = ''

      if (!this.answered && this.selectedIndex === index) {
        answerClass = 'selected'
      } else if (this.answered && this.correctIndex === index) {
        answerClass = 'correct'
      } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
        answerClass = 'incorrect'
      } else {
        answerClass = ''
      }

      return answerClass
    },

    htmlDecode(str) {
      let decoded = he.decode(str)
      return decoded
    }
  },

  mounted() {
    this.shuffleAnswers()
  }
}
</script>

<style scoped>

.question-box-container {
  margin-top: 40px; 
}
.list-group {
  margin-bottom : 15px;
}

.list-group-item:hover {
  background: #EEE;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}

.selected {
  background-color: lightblue; 
}

.correct {
  background-color: lightgreen; 
}

.incorrect {
  background-color: red; 
}

</style>