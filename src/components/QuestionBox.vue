<template>
 <div>
     <b-jumbotron>
         <template v-slot:lead>
             {{ currentQuestion.question }}
         </template>
         <hr class="my-4">
         <b-list-group>
             <b-list-group-item
                     @click="selectAnswer(index)" v-for="(answer, index) in shuffledAnswers"
                     :key="index" :class="answerClass(index)">{{ answer }}
             </b-list-group-item>
         </b-list-group>
         <b-button variant="primary" :disabled="this.selectedIndex === null || answered" @click="submitAnswer">Submit</b-button>
         <b-button v-show="showNext" variant="success" :disabled="this.selectedIndex === null || !answered" @click="next" href="#">Next</b-button>
     </b-jumbotron>
 </div>
</template>

<script>
  import _ from 'lodash'
  export default {
    name: "QuestionBox",
    props: {
      currentQuestion: Object,
      next: Function,
      increment: Function,
      showNext: Boolean
    },
    data () {
      return {
        selectedIndex: null,
        shuffledAnswers: [],
        correctIndex: null,
        answered: false
      }
    },
    methods: {
      selectAnswer(index) {
        this.selectedIndex = index;
      },
      shuffleAnswers(){
        let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
        this.shuffledAnswers = _.shuffle(answers);
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
      },
      submitAnswer() {
        let isCorrect = false;

        if (this.selectedIndex === this.correctIndex) {
          isCorrect = true
        }
        this.answered = true;
        this.increment(isCorrect)
      },
      answerClass(index) {
        let displayClass = '';

        if(!this.answered && this.selectedIndex === index) {
          displayClass = 'selected'
        } else if(this.answered && this.correctIndex === index){
          displayClass = 'correct'
        } else if(this.answered && this.selectedIndex === index && this.correctIndex !== index) {
          displayClass = 'incorrect'
        }
        return displayClass
      }
    },
    watch: {
      currentQuestion: {
        immediate:  true,
        handler() {
          this.selectedIndex = null;
          this.correctIndex = null;
          this.answered = false;
          this.shuffleAnswers()
        }
      }
    },
    computed: {
      answers() {
        return [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      }
    }
  }
</script>

<style scoped>
 .list-group {
     margin-bottom: 15px;
 }
 .list-group-item:hover {
    background: #eeeeee;
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
    background-color: lightcoral;
}
</style>
