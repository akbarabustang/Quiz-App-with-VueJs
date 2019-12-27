<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click="selectAnswer(index)"
          :class="[selectedIndex === index ? 'selected' : '']"
        >
         {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button
       variant="primary" 
       @click="submitAnswer"
       >Submit
      </b-button>

      <b-button @click="next" variant="success" href="#">Next</b-button>

    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },

  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswer: []
    }
  },

  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  },

  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
      this.selectedIndex = null
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
    },

    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswer = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswer.indexOf(this.currentQuestion.correct_answer)
    }
  },

  mounted() {
    this.shuffleAnswers()
  }
}
</script>

<style scoped>

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