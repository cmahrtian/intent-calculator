<script>
export default {
  data () {
    return {
      displayedNumber: 0,
      buttonRows: [
        ['AC', '+/-', '%', '÷'],
        [7, 8, 9, 'x'],
        [4, 5, 6, '-'],
        [1, 2, 3, '+'],
        [0, '.', '=']
      ]
    }
  },
  methods: {
    setDisplayedNumber(number) {
      if (typeof number === 'number') {
        if (this.displayedNumber === 0) {
          this.displayedNumber += number
        } else {
          this.displayedNumber = Number(
            this.displayedNumber.toString() + number.toString()
          )
        }
      }
    }
  }
}
</script>

<template>
  <div class="calculator">
    <h1>{{ displayedNumber }}</h1>
    <ul>
      <li v-for="(row, index) in buttonRows"
        :key="index">
        <span v-for="(button, index) in row"
          :key="index"
          @click="setDisplayedNumber(button)">
          <button>{{ button }}</button>
        </span>
      </li>
    </ul>
  </div>
</template>

<style lang="css" scoped>
  .calculator {
    width: 50%;
    margin: 0 auto;
  }
  
  ul {
    list-style-type: none;
    padding-left: 0;
  }

  button {
    font-size: 1.5em;
    cursor: pointer;
  }
</style>
