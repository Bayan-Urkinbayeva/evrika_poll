<template>
  <div class="quiz">
    <div class="question">
      <h1>{{question}}</h1>
      <slot></slot>
    </div>
    <div class="input">
      <div class="answer">
        <div class="answer_choice">
          <input type="radio" :id="idYes" v-bind="$attrs" />
          <label :for="idYes" @click="$emit('makeYes')">
            <i class="fas fa-check"></i>Да
          </label>
        </div>
        <div class="answer_choice">
          <input type="radio" :id="idNo" v-bind="$attrs" />
          <label :for="idNo" @click="$emit('makeNo')">
            <i class="fas fa-check"></i>Нет
          </label>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "booleanQuiz",
  inheritAttrs: false,
  props: ["idYes", "idNo", "question"],
};
</script>
