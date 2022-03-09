<template>
  <div id="app">
    <input placeholder="01/01/1970" type="text" v-model="dateInput" />
  </div>
</template>

<script>
export default {
  name: 'DateInput',
  data() {
    return {
      dateInput: this.value,
    };
  },
  props: {
    value: {
      type: [String, Date],
    },
    minYear: {
      type: String,
      required: false,
    },
  },
  watch: {
    dateInput(currentValue) {
      this.onInputChange(currentValue);
    },
  },
  methods: {
    onInputChange(currentValue) {
      if (/\D\/$/.test(currentValue)) {
        currentValue = currentValue.substr(0, currentValue.length - 3);
      }
      let values = currentValue.split('/').map(function (v) {
        return v.replace(/\D/g, '');
      });
      if (values[0]) values[0] = this.checkValue(values[0], '01', '31');
      if (values[1]) values[1] = this.checkValue(values[1], '01', '12');
      if (values[2]) {
        values[2] = this.checkValue(
          values[2],
          this.minYear ? this.minYear : '1900',
          new Date().getFullYear().toString()
        );
      }
      let output = values.map(function (v, i) {
        return v.length == 2 && i < 2 ? v + ' / ' : v;
      });
      currentValue = output.join('').substr(0, 14);
      this.dateInput = currentValue;
      console.log('inside', this.strToDate(currentValue));
      this.$emit('input', this.strToDate(currentValue));
    },
    checkValue(str, min, max) {
      const currentMin = [...min]
        .map((char, charIndex) => {
          return str[charIndex] ? str[charIndex] : '0';
        })
        .join('');
      const currentMax = [...max]
        .map((char, charIndex) => {
          return str[charIndex] ? str[charIndex] : '9';
        })
        .join('');

      if (
        (parseInt(currentMin) < parseInt(min) ||
          parseInt(currentMin) > parseInt(max)) &&
        (parseInt(currentMax) < parseInt(min) ||
          parseInt(currentMax) > parseInt(max))
      )
        str = str.slice(0, str.length - 1) + str.slice(str.length - 1 + 1);

      return str;
    },
    strToDate(str) {
      const values = str.split(' / ');
      if (values.length === 3 && values[2].length === 4) {
        return new Date(values[2], values[1] - 1, values[0]);
      }
      return null;
    },
  },
};
</script>

<style></style>
