<template>
  <div :class="{ active: isActive }" id="home">
    <Header/>
    <div class="img">
      <img src="../assets/quiz.png">
    </div>
    <div class="form">
      <div class="quiz_content">
        <booleanQuiz
          question="Приобретали ли Вы ранее товары в Evrika?"
          name="radio_1"
          v-bind="was_bought"
          idYes="radio_1"
          idNo="radio_2"
          @makeYes="was_bought = true"
          @makeNo="was_bought = false">
        <p class="visible" v-if="errors[0]=='Error'">Это поле обязательное</p> 
        </booleanQuiz>
        <booleanQuiz
          question="Хотели бы Вы вернуться к нам повторно?"
          name="radio_2"
          v-bind="will_return"
          @makeYes="will_return = true"
          @makeNo="will_return = false"
          idYes="radio_3"
          idNo="radio_4">
        <p class="visible" v-if="errors[1]=='Error'">Это поле обязательное</p> 
        </booleanQuiz>
        <Quiz
          question="Оцените работу продавца-консультанта:"
          name="radio_3"
          v-bind="seller_score"
          @changeValue="setSellerScore"
          idChoose="radio_seller">
        <p class="visible" v-if="errors[2]=='Error'">Это поле обязательное</p> 
        </Quiz>
        <Quiz
          question="Оцените работу кассира:"
          name="radio_4"
          v-bind="cashier_score"
          @changeValue="setCashierScore"
          idChoose="radio_cashier">
        <p class="visible" v-if="errors[3]=='Error'">Это поле обязательное</p> 
        </Quiz>
        <Quiz
          question="Насколько Вам приятно рекомендовать нашу компанию своим друзьям или знакомым?"
          name="radio_5"
          v-bind="recommendation_score"
          @changeValue="setRecommendationScore"
          idChoose="radio_rec">
        <p class="visible" v-if="errors[4]=='Error'">Это поле обязательное</p> 
        </Quiz>
        <div class="quiz q6">
          <div class="question">
            <h1>Пожалуйста, оставьте краткий отзыв или ваш комментарий :</h1>
            <p class="visible" v-if="errors[5]=='Error'">
              Данное поле не должно быть меньше 3 слов
            </p>
          </div>
          <div class="input">
            <textarea
              v-model="comment"
              placeholder="Пожалуйста,оставьте краткий отзыв или ваш комментарий: "></textarea>
          </div>
        </div>
        <div class="quiz_button">
          <button @click.prevent="sendComment">Отправить отзыв</button>
        </div>
      </div>
    </div>
  </div>
  <div v-show="showModal" class="modal">
    <div class="modal_checked">
      <i class="far fa-check-circle"></i>
      <p>Спасибо за отзыв!</p>
      <a href="https://evrika.com"><button @click="closeModal">Закрыть</button></a>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import booleanQuiz from "./booleanQuiz.vue";
import Quiz from "./Quiz.vue";
import Header from "./Header.vue"
export default {
  name: "Home",
  components: {
    booleanQuiz,
    Quiz,
    Header
  },
  data() {
    return {
      was_bought: null,
      will_return: null,
      seller_score: null,
      cashier_score: null,
      recommendation_score: null,
      comment: "",
      showModal: false,
      isActive: false,
      errors: [],
    };
  },
  methods: {
    async sendComment() {
       this.errors = [this.isBoolean(this.was_bought),this.isBoolean(this.will_return), this.isChoosen(this.seller_score),this.isChoosen(this.cashier_score), this.isChoosen(this.recommendation_score),this.checkComment(this.comment)]
      if (
        this.was_bought != null && this.will_return != null && this.seller_score != null && this.cashier_score != null && this.recommendation_score != null &&
        (this.comment == "" ||
          (this.comment.length >= 3 && this.comment.length <= 500))
      ) {
        await this.fetchData();
      }
    },
    async fetchData() {
      const req_data = {
        was_bought: this.was_bought,
        will_return: this.will_return,
        seller_score: this.seller_score,
        cashier_score: this.cashier_score,
        recommendation_score: this.recommendation_score,
        comment: this.comment,
      };
      await axios
        .put(`http://10.10.1.48:3245/api/polls/kdNQ`, req_data)
        .then((res) => {
          if (res.status == 201) {
            this.showModal = true;
            this.isActive = true;
          } else {
            window.location.href = "https://evrika.com";
          }
        })
    },
    
    getUrl() {
      let arr = window.location.pathname;
      let hash = arr.pop();
      this.hash = hash;
    },

    setSellerScore(score) {
      return (this.seller_score = score);
    },

    setCashierScore(score){
      return (this.cashier_score = score)
    },

    setRecommendationScore(score){
      return (this.recommendation_score = score)
    },
        isChoosen(value) {
      return !value ? "Error" : null;
    },

    checkComment(value) {
      if (value != "" && (value.length < 3 || value.length > 500)) {
        return "Error";
      } else {
        return null;
      }
    },
    isBoolean(value){
      if(typeof value === 'boolean'){
        return null
      }
      else{
        return "Error"
      }
    }
  },
  //   created() {
  //     this.getUrl();
  //   },
};
</script>
<style lang="scss">
@import "../style/style.scss";
</style>