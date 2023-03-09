<script setup>
import { ref } from 'vue';
import APODContainer from '../components/APODContainer.vue';

let selectedDate = ref(null);
</script>

<template>
  <div class="home">
    <div class="left-bar">
      <h2>Select a Date</h2>
      <input
        v-model="selectedDate"
        min="1995-06-01"
        max="2100-01-01"
        id="date-selector"
        type="date"
      />
    </div>
    <APODContainer class="apod-container" :selected-date="selectedDate" />
  </div>
</template>

<style scoped>
.home {
  display: flex;
}

.apod-container {
  width: 70%;
  margin: 40px;
  box-shadow: #cccccc 0px 0px 10px 0px;
  /* max-height: calc(100vh - 80px); */
}

.left-bar {
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: center;
  padding: 30px 0px;
  height: fit-content;
  background-color: #ffffff;
  box-shadow: #cccccc 0px 0px 10px 0px;
  border-radius: 10px;
  margin: 40px;
  width: 30%;
}

.search-button {
  width: calc(100% - 60px);
  color: white;
  font-weight: bold;
  background-color: #8036bc;
  padding: 14px;
  font-size: 1em;
  border-radius: 5px;
  border: none;
  transition: background-color 0.2s;
}
.search-button:hover {
  background-color: #8c37d6;
  cursor: pointer;
  transition: background-color 0.2s;
}

input[type='date'] {
  cursor: text;
  font-size: 1.2em;
  border: none;
  outline: none;
  background-color: #eee;
  border-radius: 5px;
  width: calc(100% - 60px);
  text-align: center;
  font-family: 'Century Gothic';
  font-weight: bold;
  /* margin: 20px; */
}
::-webkit-datetime-edit {
  padding: 14px 20px;
  background-color: #eee;
}
::-webkit-datetime-edit-fields-wrapper {
  background-color: #eee;
}
::-webkit-datetime-edit-text {
  padding: 0 0.3em;
}
::-webkit-datetime-edit-text,
::-webkit-datetime-edit-month-field,
::-webkit-datetime-edit-day-field,
::-webkit-datetime-edit-year-field {
  color: #a2a1a1;
  /* background-color: transparent; */
}

::-webkit-datetime-edit-month-field:focus,
::-webkit-datetime-edit-day-field:focus,
::-webkit-datetime-edit-year-field:focus {
  background-color: #ab7dca;
  border-radius: 2px;
  color: white;
  /* background-color: transparent; */
}
::-webkit-calendar-picker-indicator {
  margin-right: 15px;
  cursor: pointer;
}
::-webkit-inner-spin-button {
  display: none;
}
</style>
