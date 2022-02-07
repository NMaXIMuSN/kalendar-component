<template>
  <input
    @keyup.enter="enterClick"
    type="text"
    class="calendar__input"
    v-model="inputData"
    placeholder="yyyy-mm-dd"
  />
</template>

<script>
export default {
  data: () => ({
    inputData: "",
  }),
  methods: {
    enterClick() {
      const data = this.inputData.split("-");
      data[1]--;
      if (this.cheackData) this.$emit("newCurrentData", data);
      else console.log("бэд дата");
    },
  },

  computed: {
    cheackData() {
      const reg = /\d*\d*\d*\d*-1*\d-[123]*\d/gm;
      const day = this.inputData.split("-")[2];
      const month = this.inputData.split("-")[1] - 1;
      const yaer = this.inputData.split("-")[0];

      return (
        reg.test(this.inputData) &&
        day == new Date(yaer, month, day).getDate() &&
        month == new Date(yaer, month, day).getMonth() &&
        yaer == new Date(yaer, month, day).getFullYear()
      );
    },
  },
};
</script>

<style lang="scss" scoped>
.calendar__input {
  outline: none;
  width: 300px;
  padding: 10px 10px;
  font-size: 16px;
}
</style>
