<script setup>
import { ref, reactive, watch, onMounted, defineProps } from 'vue';
import loadingImg from '../assets/loading.svg';

const props = defineProps({
  selectedDate: String,
});

let data = reactive({});
let loading = ref(true);
let success = ref(true);

async function loadData() {
  loading.value = true;
  data.hdurl = loadingImg;

  await fetch(
    `https://api.nasa.gov/planetary/apod?api_key=Ww0Xuo2kdnZyguPsJdummwwc7aW1I4M3XboVoHeY${
      props.selectedDate ? '&date=' + props.selectedDate : ''
    }`
  )
    .then(response => response.json())
    .then(json => {
      if (json.code == 400) {
        loading.value = false;
        success.value = false;
        return;
      }
      data = json;
      success.value = true;
      loading.value = false;
    });
}

watch(
  () => props.selectedDate,
  async newValue => {
    const date = new Date(newValue);
    if (
      date.valueOf() != NaN &&
      date.getFullYear() > 1994 &&
      date.getFullYear() < new Date().getFullYear()
    ) {
      loadData();
    }
  }
);

onMounted(() => {
  loadData();
});
</script>

<template>
  <div class="apod-container">
    <div v-if="success" class="info-container">
      <h1 class="title">{{ data.title }}</h1>
      <span class="description">{{ data.explanation }}</span>
      <img id="apod-img" :src="data.hdurl" />
      <b>{{ data.date }}</b>
    </div>
    <div class="error-message" v-else>
      <h1 class="error-title">
        Sorry there is no APOD of this date, please try with another one.
      </h1>
    </div>
  </div>
</template>

<style scoped>
.apod-container {
  background-color: #ffffff;
  height: calc(100vh - 100px);
  display: flex;
  border-radius: 10px;
}

.error-message {
  width: 100%;
  height: 100%;
  justify-content: center;
  align-items: center;
  display: flex;
}
.error-message .error-title {
  width: 500px;
  text-align: center;
}

.info-container {
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 100px;
  justify-content: center;
  align-items: center;
}
.title {
  width: 100%;
}
.description {
  color: #7c7c7c;
  text-align: justify;
}

#apod-img {
  border-radius: 10px;
  height: 350px;
  /* border: 1px #1f1f1f solid; */
}
</style>
