<template>
  <div class="question-box-container">
    
    <b-jumbotron>
      <template slot="lead">{{currentQuestion.question}}</template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(ans,index) in addanswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="addClass(index)"
        >{{ ans }}</b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedindex==null|| answered"
      >Submit</b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>
<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: new Object(),
    next: Function,
    increment: Function
   },
  data() {
    return {
      selectedindex: null,
      shuffledAnswer: [],
      correctIndex: null,
      answered: false
    };
  },
  computed: {
    addanswers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  watch: {
    // currentQuestion(){
    //   this.selectedindex =null
    //   this.shuffleAnswer()
    // }
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedindex = null;
        this.answered = false;
        this.shuffleAnswer();
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedindex = index;
    },
    submitAnswer() {
      let Iscorrect = false;
      if (this.selectedindex == this.correctIndex) {
        Iscorrect = true;
      }
      this.answered = true;
      this.increment(Iscorrect);
    },
    shuffleAnswer() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.shuffledAnswer = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswer.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    addClass(index) {
      let answerclass = "";
      if (!this.answered && this.selectedindex == index) {
        answerclass = "selected";
      } else if (this.answered && this.correctIndex == index) {
        answerclass = "correct";
      } else if (
        this.answered &&
        this.selectedindex == index &&
        this.correctIndex !== index
      ) {
        answerclass = "incorrect";
      }
      return answerclass;
    }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background: #eee;
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


