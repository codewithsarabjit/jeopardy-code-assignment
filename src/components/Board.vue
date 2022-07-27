<template>
  <article>
    <div class="row headstack align-items-start">
        <div class="col" v-for="(category, ckey) in categories" :key="ckey">{{category}}</div>
    </div>
    <div class="row g-1 align-items-start">
        <div class="col" v-for="(categoryName, ckey) in categories" :key="ckey">
        <div class="vstack gap-1">
            <div class="" v-for="(question, qkey) in questions.filter(({category})=>category==categoryName).slice(0, 5)" :key="qkey" @click="$emit('selectedQuestion', question)" :class="[answered.includes(question.question)?'disabled':'']">{{question.value}}</div>
        </div>
        </div>
    </div>
  </article>
</template>
<script>
export default {
    props: ['categories', 'questions', 'answered'],
    emits: ['selectedQuestion']
}
</script>
<style lang="scss" scoped>
.vstack {
  div {
    border: 2px solid gray;
    padding: 10px;
    &.disabled{
      pointer-events: none;
      background: #dedede;
    }
    &:hover{
      background: #0d6efd;
      color: white;
    }
  }
}
.headstack {
  .col {
    font-weight: bold;
  }
}
</style>