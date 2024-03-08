<template>
  <q-page class="row items-center justify-center">
    <q-card class="column items-center q-mb-md" style="max-width: 800px; background-color: rgba(255, 255, 255, 0.5); border-radius: 10px;">
      <q-card-section class="justify-center text-center">
        <p class="text-h6 question pink-text">{{ questionText }}</p>
        <q-img :src="imageUrl" style="width: 300px; height: 200px;"/>
      </q-card-section>
      <q-card-actions class="q-mt-md" v-if="!answered">
        <q-btn text-color="white" class="q-my-sm pink-bg" style="width: 150px; height: 50px; border-radius: 25px;" @click="answerYes">Да</q-btn>
        <q-btn @click="showNotify()" text-color="white" class="q-my-sm pink-bg" style="width: 150px; height: 50px; border-radius: 25px; margin-top: 10px;" :class="{ 'hide-on-hover': isHover }" @mouseover="isHover = true" @mouseout="isHover = false">Нет</q-btn>
      </q-card-actions>
      <q-card-actions class="q-mt-md" v-if="answered && showResetButton">
        <q-btn text-color="white" class="q-my-sm pink-bg" style="width: 150px; height: 50px; border-radius: 25px;" @click="reset">Начать сначала</q-btn>
      </q-card-actions>
    </q-card>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useQuasar } from 'quasar';

const initialImageUrl = 'https://media1.tenor.com/m/H2GvCmfShZEAAAAC/menhera-chan.gif';
const initialQuestionText = 'Пойдешь гулять со мной?';

const imageUrl = ref(initialImageUrl);
const questionText = ref(initialQuestionText);
const answered = ref(false);
const isHover = ref(false);
const showResetButton = ref(false); // Добавляем реактивную переменную для отображения кнопки
const $q = useQuasar();

const answerYes = () => {
  imageUrl.value = 'https://media1.tenor.com/m/ZINgFAwKh1QAAAAC/anime-love.gif';
  questionText.value = 'УРАААААА!';
  answered.value = true;

  // Показываем кнопку через 2-3 секунды
  setTimeout(() => {
    showResetButton.value = true;
  }, 2000); // 2 секунды
};

const showNotify = () => {
  $q.notify({
    color: 'pink-5',
    message: `
      <div style="text-align: center;">
        <em>НЕПРАВИЛЬНЫЙ ВЫБОР</em><br>
        <img src="https://media.tenor.com/sCZ6fh4G1CIAAAAi/cry-crying.gif" alt="cry" style="width: 200px; height: 150px"/>
      </div>
    `,
    position: 'center',
    html: true,
    timeout: 1000
  });
};


const reset = () => {
  imageUrl.value = initialImageUrl;
  questionText.value = initialQuestionText;
  answered.value = false;
  showResetButton.value = false; // Скрыть кнопку после сброса
};
</script>

<style scoped>
.pink-text {
  color: #ffc0cb;
}

.pink-bg {
  background: #ffc0cb;
}

.hide-on-hover {
  animation: hideButton 0.5s forwards;
}

@keyframes hideButton {
  0% {
    opacity: 1;
    transform: translateY(0);
  }
  100% {
    opacity: 0;
    transform: translateY(50px);
  }
}
</style>
