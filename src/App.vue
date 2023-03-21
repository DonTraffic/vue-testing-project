<template>
  <div class="main">
    <h1 class="main__title">
      Мобильным приложением "Онлайн", легко пользоваться
    </h1>
    <sub class="main__sub-title">
      (где 1-абсолютно не согласен, а 7-абсолютно согласен)
    </sub>

    <RatingSlider @returnValue="getValue"/>

    <template v-if="sliderValue && sliderValue < 6">
      <p class="main__text">
        Уточните, какой именно функционал требует улучшения в интернет-банке?
      </p>
      <sub class="main__sub-title">
        (возможно несколько вариантов)
      </sub>

      <div class="main__questions">
        <QuestionItem
          :info="question"
          :questionId="index"
          v-for="(question, index) in questionsData"
          :key="question.id"
          @changeQuestion="getQuestion"
        />
      </div>
    </template>

    <button class="main__button" @click="submitBtn()">Далее</button>

  </div>
</template>

<script>
import QuestionItem from './components/question.vue';
import RatingSlider from './components/rating.vue';

export default {
  name: "App",

  components: {RatingSlider, QuestionItem},

  data() {
    return {
      sliderValue: '',

      questionsStatus: {},

      questionsData: {
        0: {
          title: 'Информация по вашим продуктам (Счета, карты, вклады, кредиты и т.д)',
          answers: ["Текущий счёт", "Накопительный счёт", "Дебетовая карта", "Вклады", "Ипотека", "Кредит"]
        },

        1: {
          title: 'Обплата услуг (ЖКХ, мобильная связь, интернет и т.д)',
          answers: ["ЖКХ", "Мобильная связь", "Интернет", "Другие услуги"]
        },

        2: {
          title: 'Переводы',
          answers: ["Переводы между своими счетами", "Обмен валюты", "Переводы по номеру телефона", "Переводы по номеру карты", "Переводы по реквизитам", "SWIFT-переводы"]
        },

        3: {
          title: 'Другое',
          answers: ["Настройки приложения", "Информация о тарифах"]
        },

      }

    }
  },

  methods: {
    getValue(value) {
      this.sliderValue = value
    },

    getQuestion(data) {
      this.questionsStatus[data.id] = {
        id: data.id,
        status: data.status
      }
    },

    submitBtn() {
      let status = true

      if (this.sliderValue < 6) {
        for (const key in this.questionsStatus) {
          if (!this.questionsStatus[key].status) {
            window.scrollTo({
              top: 0, // document.getElementById('question-' + this.questionsStatus[key].id).getBoundingClientRect()
              behavior: "smooth",
            });
            status = false
          }

        }

      }

      if (status) alert('Данные отправлены, спасибо за обратную связь')

    }

  }

};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  outline: none;
}

.main {
  max-width: 420px;
  width: 100%;
  margin: 0 auto;
  padding: 8px;
  text-align: center;

  &__title {
    color: rgb(209, 30, 6);
    margin-bottom: 4px;
  }

  &__sub-title {
    color: rgb(119, 119, 119);
  }

  &__text {
    color: rgb(49, 49, 49);
    font-weight: bold;
    font-size: 20px;
  }

  &__questions {
    margin-top: 24px;
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  &__button {
    margin-top: 32px;
    width: 100%;
    background-color: rgb(209, 30, 6);
    color: #fff;
    padding: 12px;

    border: 0px;
    border-radius: 6px;

    cursor: pointer;
  }
}
</style>
