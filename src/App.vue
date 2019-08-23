<template>
  <div id="app">
    <Header :numcorrect="numcorrect" :numTotal="numTotal" />

    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <Questionbox
            v-if="questions.length"
            :currentQuestion="questions[this.index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Questionbox from "./components/Questionbox.vue";

export default {
  name: "app",
  components: {
    Header,
    Questionbox
  },
  methods: {
    next() {
      this.index++;
    },
    increment(iscorrect) {
      if (iscorrect) {
        this.numcorrect++;
      }
      this.numTotal++;
    }
  },
  data() {
    return {
      questions: [],
      index: 0,
      numcorrect: 0,
      numTotal: 0
    };
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&category=27&type=multiple", {
      method: "get"
    })
      .then(response => {
        return response.json();
      })
      .then(jsonData => {
        var self = this;
        self.questions = jsonData.results;
      });
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
