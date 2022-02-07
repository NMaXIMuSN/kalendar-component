<template>
  <div class="calendar">
    <div class="calendar__head">
      <img
        src="@/assets/arrow.svg"
        alt="-"
        @click="backMonth"
        class="calendar__arrow calendar__arrow-left"
      />
      <div class="calendar__title">{{ monthHead }}</div>
      <img
        src="@/assets/arrow.svg"
        alt="+"
        @click="nextMonth"
        class="calendar__arrow calendar__arrow-right"
      />
    </div>
    <div class="calendar__week">
      <div class="calendar__week-day" v-for="name in nameWeekDay" :key="name">
        {{ name }}
      </div>
    </div>
    <div class="calendar__main">
      <div
        class="calendar__main-day"
        @click="$emit('newFocusDay', getIntTime(day))"
        :class="{ 'calendar__main-day-focus': getIntTime(day) == focusDate }"
        v-for="day in getDayMain"
        :key="timeInt(day)"
      >
        {{ getOnlyDay(day) }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({}),

  props: {
    currentData: {
      type: Date,
    },
    isEnglish: {
      type: Boolean,
      default: false,
    },

    focusDate: {
      type: Number,
    },
  },

  computed: {
    monthHead() {
      const data = new Date(this.currentData);
      const month = data.getMonth();
      const monthName = this.isEnglish
        ? [
            "January",
            "February",
            "March",
            "April",
            "May",
            "June",
            "July",
            "August",
            "September",
            "October",
            "November",
            "December",
          ]
        : [
            "Январь",
            "Февраль",
            "Март",
            "Апрель",
            "Май",
            "Июнь",
            "Июль",
            "Август",
            "Сентябрь",
            "Октябрь",
            "Ноябрь",
            "Декабрь",
          ];

      return `${monthName[month]}  ${data.getFullYear()}`;
    },

    nameWeekDay() {
      return this.isEnglish
        ? ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"]
        : ["НП", "ВТ", "СР", "ЧТ", "ПТ", "СБ", "ВС"];
    },

    getDayMain() {
      const data = new Date(this.currentData);
      const currentMonth = data.getMonth();
      const currentYear = data.getFullYear();
      const firstDay = new Date(currentYear, currentMonth, 1);
      const lastDay =
        new Date(data).setMonth(currentMonth + 1) -
        data.getDate() * 24 * 3600 * 1000;

      const monthDays = [];
      for (let index = 1; index <= new Date(lastDay).getDate(); index++) {
        monthDays.push(new Date(currentYear, currentMonth, index));
      }

      let dayWeek = this.isEnglish
        ? new Date(firstDay).getDay()
        : new Date(firstDay).getDay() !== 0
        ? new Date(firstDay).getDay() - 1
        : 7 - 1;

      while (dayWeek > 0) {
        monthDays.unshift("");
        dayWeek -= 1;
      }

      return monthDays;
    },
  },

  methods: {
    getOnlyDay(data) {
      return data === "" ? "" : new Date(data).getDate();
    },

    getIntTime(day) {
      return new Date(day).getTime();
    },

    timeInt(data) {
      return data === "" ? Math.random() : new Date(data).getTime();
    },

    nextMonth() {
      const data = new Date(this.currentData);
      const currentMonth = data.getMonth();
      const currentYear = data.getFullYear();
      const currentData = new Date(currentYear, currentMonth + 1, 1);
      this.$emit("updateDate", currentData);
    },
    backMonth() {
      const data = new Date(this.currentData);
      const currentMonth = data.getMonth();
      const currentYear = data.getFullYear();
      const currentData = new Date(currentYear, currentMonth - 1, 1);
      this.$emit("updateDate", currentData);
    },
  },
};
</script>

<style lang="scss" scoped>
.calendar {
  width: 300px;
  height: 300px;
  border: 1px solid #000000;
  background-color: #e6e6e6;

  &__head {
    display: flex;
    justify-content: space-between;
    padding: 10px;
    border-bottom: 1px solid #000000;
  }

  &__arrow {
    width: 20px;
    height: 20px;
    &-left {
      transform: rotate(180deg);
    }
  }
  &__week {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    padding: 5px 10px;

    &-day {
      text-align: center;
    }
  }

  &__main {
    padding: 0 10px;
    display: grid;
    grid-template-columns: repeat(7, 1fr);

    &-day {
      height: 39px;
      display: flex;
      justify-content: center;
      align-items: center;

      &-focus {
        border: 1px solid blue;
      }
    }
  }
}
</style>
