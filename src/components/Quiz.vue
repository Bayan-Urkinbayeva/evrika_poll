<template>
<div class="quiz">
    <div class="question">
        <h1>{{question}}</h1>
        <p>Оценка от 1 до 5, где 1 «очень плохо», а 5 «очень хорошо»</p>
        <slot></slot>
    </div>
    <div class="input">
        <div class="choice">
            <label v-for="(score, index) in scores" :key="index" :for="idChoose+score">
            <input @click="$emit('changeValue', score)" :id="idChoose+score" type="radio" v-bind="$attrs">
            <span>  {{ score }} </span>
            </label>
        </div>
    </div>
</div>
</template>
<script>
export default {
    name: "Quiz",
    props: ["question", 'idChoose'],
    data(){
        return{
            scores: [1, 2, 3, 4, 5],
        }
    }
}
</script>
