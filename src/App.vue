<template>
  <div id="app">
    <span class="checkbox__wrapper">
      <input id="checkbox" v-model="englishLang" type="checkbox" />
      <label for="checkbox">Английский язык</label>
    </span>
    <my-input @newCurrentData="inputDate" />
    <calendar
      :currentData="currentData"
      :isEnglish="englishLang"
      :focusDate="focusDate"
      @newFocusDay="newFocusDay"
      @updateDate="updateDate"
    />
  </div>
</template>

<script>
import calendar from "./components/calendar.vue";
import MyInput from "./components/myInput.vue";

export default {
  data: () => ({
    currentData: null,
    englishLang: false,
    focusDate: null,
  }),
  components: { calendar, MyInput },
  name: "App",
  methods: {
    updateDate(value) {
      this.currentData = value;
    },
    inputDate(value) {
      this.currentData = new Date(value[0], value[1], value[2]);
      this.focusDate =  new Date(value[0], value[1], value[2]).getTime()
    },
    newFocusDay(value) {
      this.focusDate = value;
    },
  },
  mounted() {
    const data = new Date();
    const currentMonth = data.getMonth();
    const currentYear = data.getFullYear();
    this.currentData = new Date(currentYear, currentMonth, 1);
  },
};
</script>

<style lang="scss">
*,
*::after,
*::before {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

#app {
  font-family: Arial, Helvetica, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  gap: 15px;
  min-width: 100vw;
  min-height: 100vh;
  background-color: #1e1e1e;
}

.checkbox__wrapper {
  color: #fff;
  label {
    padding-left: 15px;
  }
}
</style>
