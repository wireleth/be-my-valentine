<template>
  <div class="dialog-overlay">
    <div class="dialog-box">
      <h2 class="question pb-0 mb-0">{{ question }}</h2>
      <div v-if="!loaded" class="image-placeholder text-center">
        ⏳ Загрузка...
      </div>
      <ClientOnly>

      <Transition name="fade" appear>
        <img
          :src="questionImg"
          class="img-fluid question-image"
          @load="loaded = true"
          :class="{ 'fade-in': loaded }"
          alt=""
        />
      </Transition>
      </ClientOnly>
      <div v-if="!cookieAnswer" class="buttons">
        <button class="mx-2 button-yes" @click="handleAnswer('да')">Да!</button>
        <button class="mx-2 button-no" @click="handleAnswer('нет')">
          Нет.
        </button>
      </div>
      <div v-if="cookieAnswer" class="buttons">
        <button
          class="mx-2 button-yes"
          @click="
            () => {
              (cookieAnswer = ''), (questionImg = 'tell-me.png');
            }
          "
        >
          Я передумала! 😓
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const loaded = ref(false);
const resolveQuestionImg = (answer: string | null | undefined) => {
  if (answer == "да") {
    return "yes.jpg";
  } else if (answer == "нет") {
    return "dark-souls-defeated.gif";
  }
  return "tell-me.png";
};

const questionImg = ref("");
onMounted(() => {
  questionImg.value = resolveQuestionImg(cookieAnswer.value);
});
const isVisible = ref(true);

const question = "Ты будешь моей Марин?";

const cookieId = useCookie<string>("cookieId");
cookieId.value = cookieId.value || crypto.randomUUID();
const cookieAnswer = useCookie("answer");
const handleAnswer = async (answer: string) => {
  questionImg.value = resolveQuestionImg(answer);
  cookieAnswer.value = answer;
  const { data, error } = useFetch(
    `${useRuntimeConfig().public.API_URL}/send`,
    {
      method: "POST",
      body: {
        uuid: cookieId.value,
        answer: cookieAnswer.value,
      },
    }
  );
  isVisible.value = true;
};
</script>

<style scoped>
.dialog-overlay {
  display: flex;
  justify-content: center;
  align-items: center;
}

.dialog-box {
  text-align: center;
  max-width: 400px;
  width: 100%;
}

.question {
  font-size: 22px;
  margin-bottom: 20px;
}
.buttons {
  display: flex;
  flex-direction: row;
  margin-left: 1rem;
  margin-right: 1rem;
}
.buttons button {
  width: 100%;
  font-size: 16px;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}
.button-yes {
  color: #fff;
  background: #ff4081;
  border: none;
}
.button-no {
  color: #000;
  background: none;
  border-style: solid;
  border-color: #ff4081;
  border-width: 1px;
}

.buttons button:hover {
  background: #ff80ab;
}
.response {
  text-decoration: underline;
}
.curvy-text {
  font-weight: bold;
  position: relative;
  display: inline-block;
}

.question-image {
  opacity: 0;
  transition: all 0.5s ease-in-out;
  width: 100%;
  height: 327px;
  object-fit: cover;
  background: #ff80aa4d;
  -webkit-mask-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0) 10%,
      rgba(0, 0, 0, 1) 70%
    ),
    linear-gradient(to top, rgba(0, 0, 0, 0) 30%, rgba(0, 0, 0, 1) 80%);
  -webkit-mask-composite: destination-in;
  mask-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0) 0%,
      rgba(0, 0, 0, 1) 20%
    ),
    linear-gradient(to top, rgba(0, 0, 0, 0) 1%, rgba(0, 0, 0, 1) 50%);
  mask-composite: intersect;
}
img.fade-in {
  opacity: 1;
}
.image-placeholder {
  height: 327px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 18px;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease-in-out;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
.fade-enter-active {
  transition: opacity 0.5s ease-in-out, filter 0.5s ease-in-out;
}
.fade-enter-from {
  opacity: 0;
  filter: blur(10px);
}
</style>
