<template>
  <article>
    <h5>{{question.question}}</h5>
    <ul>
        <li v-for="(answer, akey) in question.answers" :key="akey" @click="$emit('selectAnswer', question, answer, akey)" :class="[selectedAnswer === akey ? (correctAnswer===answer ? 'success' : 'danger') : '']">{{answer}}</li>
    </ul>
    <div v-if="questionSelected" @click="$emit('backToQuestionList')" class="btn btn-outline btn-danger">back</div>
  </article>
</template>
<script>
export default {
    props: ['correctAnswer', 'question', 'selectedAnswer', 'questionSelected'],
    emits: ['backToQuestionList', 'selectAnswer']
}
</script>
<style lang="scss" scoped>
ul {
  list-style: none;
  padding: 0;
  li {
    text-align: left;
    border: 2px solid gray;
    padding: 2px 10px;
    margin-bottom: 5px;
    &.success{
      background: #198754;
      color: white;
    }
    &.danger{
      background: #dc3545;
      color: white;
    }
    &:hover{
      background: #0d6efd;
      color: white;
    }
  }
}
</style>