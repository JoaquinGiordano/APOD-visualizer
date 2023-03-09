<script>
import { ref, reactive } from 'vue';
import loading from '../assets/loading.svg';
export default {
  props: {
    selectedDate: String,
  },

  setup() {
    const data = reactive({});
    const loading = ref(true);
    const success = ref(true);

    async function loadData() {
      this.loading = true;
      this.data.hdurl = loading;
      await fetch(
        `https://api.nasa.gov/planetary/apod?api_key=Ww0Xuo2kdnZyguPsJdummwwc7aW1I4M3XboVoHeY${
          this.selectedDate ? '&date=' + this.selectedDate : ''
        }`
      )
        .then(response => response.json())
        .then(json => {
          if (json.code == 400) {
            this.loading = false;
            this.success = false;
            return;
          }

          this.data = json;
          this.success = true;
          this.loading = false;
        });
    }
  },
  watch: {
    selectedDate(newValue) {
      const date = new Date(newValue);
      if (
        date.valueOf() != NaN &&
        date.getFullYear() > 1994 &&
        date.getFullYear() < new Date().getFullYear()
      ) {
        this.loadData();
      }
    },
  },

  mounted() {
    this.loadData();
  },
};
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
