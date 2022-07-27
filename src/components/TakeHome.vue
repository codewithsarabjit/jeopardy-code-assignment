<template>
  <article>
    <h1>Jeopardy!</h1>
    <!-- Your Component(s) here! -->
    <div class="container">
      <div class="bg-primary text-white py-2 my-3">
        Hello Player! Your Score is: <span>{{score}}</span>
      </div>
      <div v-if="!this.questionSelected">
        <Board :categories="categories" :questions="questions" :answered="answered" @selectedQuestion="selectQuestion" />
      </div>
      <div v-else>
        <Question :correctAnswer="correctAnswer" :question="question" :selectedAnswer="selectedAnswer" :questionSelected="questionSelected" @backToQuestionList="backToQuestionList" @selectAnswer="selectAnswer" />
      </div>
    </div>
  </article>
</template>

<script>
  import JeopardyGame from '../mixins/Jeopardy';
  import Board from './Board.vue';
  import Question from './Question.vue';

  export default {
    name: 'TakeHome',
    components: {
      Board,
      Question
    },
    mixins:[JeopardyGame],
    data() {
      return {
        questions: [],
        answered: [],
        question: {},
        correctAnswer: null,
        selectedAnswer: null,
        score: 0
      };
    },
    computed: {
      questionSelected() {
        return Object.keys(this.question).length===0 ? false : true;
      }
    },
    methods: {
      async getQuestions() {
        const response = await this.fetchQuestions();
        this.questions = response;
      },
      selectQuestion(question) {
        this.question = question;
      },
      backToQuestionList() {
        this.question = {};
        this.correctAnswer = null;
        this.selectedAnswer = null;
      },
      async selectAnswer(question, answer, akey) {
        if (this.selectedAnswer !== null) {
          return;
        }
        const response = await this.checkAnswer(question.question, answer);
        this.selectedAnswer = akey;
        this.answered.push(question.question);
        if (response) {
          this.correctAnswer = answer;
          this.score += parseInt(question.value.replace("$", ""));
        } else {
          this.score -= parseInt(question.value.replace("$", ""));
        }
      },
    },
    beforeMount() {
      this.getQuestions();
    }
  }
</script>
<style lang="scss" scoped>
.container{
  width: 600px;
  margin: 0 auto;
}
</style>
