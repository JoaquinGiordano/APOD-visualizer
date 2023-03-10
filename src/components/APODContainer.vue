<script setup>
import { ref, reactive, watch, onMounted } from 'vue';
import moment from 'moment';
import loadingImg from '../assets/loading.svg';

let data = reactive({});
let loading = ref(true);
let success = ref(true);
let selectedDate = ref(moment(new Date()).format('YYYY-MM-DD'));
async function loadData() {
  loading.value = true;
  data.hdurl = loadingImg;
  await fetch(
    `https://api.nasa.gov/planetary/apod?api_key=Ww0Xuo2kdnZyguPsJdummwwc7aW1I4M3XboVoHeY${
      selectedDate.value ? '&date=' + selectedDate.value : ''
    }`
  )
    .then(response => response.json())
    .then(json => {
      if (json.code == 400) {
        loading.value = false;
        success.value = false;
        return;
      }

      Object.assign(data, data, json);
      success.value = true;
      loading.value = false;
    });
}
watch(
  () => selectedDate.value,
  async newValue => {
    const date = new Date(newValue);
    const actualDate = new Date();
    if (
      date.valueOf() != NaN &&
      date >= new Date('1995-07-01') &&
      date <= actualDate
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
    <input
      v-model="selectedDate"
      min="1995-06-01"
      max="2100-01-01"
      id="date-selector"
      type="date"
    />
    <div v-if="success" class="info-container">
      <div id="left-container">
        <h1 class="title">{{ data.title }}</h1>
        <span class="description">{{ data.explanation }}</span>
      </div>
      <div id="right-container">
        <img id="apod-img" :src="data.hdurl" />
        <!-- <b>{{ data.date }}</b> -->
      </div>
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
  min-height: calc(100vh - 80px);
  display: flex;
  flex-direction: column;
  border-radius: 10px;
  align-items: center;
}

#left-container,
#right-container {
  /* width: 50%; */
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
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
  height: 100%;
  gap: 60px;
  padding: 40px;
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
  width: 100%;
  max-height: 600px;
  object-fit: cover;
  /* border: 1px #1f1f1f solid; */
}

::-webkit-datetime-edit {
  color: white;
  padding: 14px 20px;
  background: linear-gradient(
    45deg,
    rgb(161, 117, 233) 0%,
    rgb(122, 70, 205) 100%
  );
}

::-webkit-datetime-edit-text {
  padding: 0 0.3em;
}
::-webkit-datetime-edit-text,
::-webkit-datetime-edit-month-field,
::-webkit-datetime-edit-day-field,
::-webkit-datetime-edit-year-field {
  color: white;
}

::-webkit-datetime-edit-month-field:focus,
::-webkit-datetime-edit-day-field:focus,
::-webkit-datetime-edit-year-field:focus {
  background-color: rgba(163, 105, 255, 0.2);
  border-radius: 2px;
  color: white;
}
::-webkit-calendar-picker-indicator {
  margin-right: 15px;
  cursor: pointer;
  filter: invert();
}
::-webkit-inner-spin-button {
  display: none;
}

input[type='date'] {
  color: white;
  background: rgb(122, 70, 205);
  cursor: text;
  font-size: 1.2em;
  border: none;
  outline: none;
  border-radius: 0px 0px 10px 10px;
  flex-shrink: 0;
  width: 60%;
  text-align: center;
  font-family: 'Century Gothic';
  font-weight: bold;
}
@media screen and (min-width: 730px) {
  .info-container {
    padding: 100px;
  }
}
@media screen and (min-width: 1300px) {
  .info-container {
    flex-direction: row;
  }

  #left-container,
  #right-container {
    width: 50%;
  }
}
</style>
