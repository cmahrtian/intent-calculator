<script>
export default {
  name: 'Calculator',
  data () {
    return {
      buttonRows: [
        ['AC', '+/-', '%', '÷'],
        [7, 8, 9, 'x'],
        [4, 5, 6, '-'],
        [1, 2, 3, '+'],
        [0, '.', '=']
      ],
      operands: ['x', '-', '+', '÷'],
      displayedNumber: 0,
      baseNumber: null,
      modifyingNumber: null,
      operandSelected: null
    }
  },
  methods: {
    buttonPressed (button) {
      if (typeof button === 'number') {
        this.setDisplayedNumber(button)
      } else if (this.operands.indexOf(button) !== -1) {
        if (!this.baseNumber) {
          this.baseNumber = this.displayedNumber
        } else {
          switch (this.operandSelected) {
            case 'x':
              this.baseNumber = this.baseNumber * this.modifyingNumber
              break;
            case '-':
              this.baseNumber = this.baseNumber - this.modifyingNumber
              break;
            case '+':
              this.baseNumber = this.baseNumber + this.modifyingNumber
              break;
            case '÷':
              this.baseNumber = this.baseNumber / this.modifyingNumber
              break;
          }
          this.displayedNumber = this.baseNumber
          this.modifyingNumber = 0 
        }
        this.operandSelected = button
      }
    },
    setDisplayedNumber (number) {
      if (this.displayedNumber === 0) {
        this.displayedNumber = number
      } else {
        if (this.operandSelected) {
          if (this.modifyingNumber && this.modifyingNumber !== 0) {
            this.modifyingNumber = Number(
              this.modifyingNumber.toString() + number.toString()
            )
          } else {
            this.modifyingNumber = number
          }
          this.displayedNumber = this.modifyingNumber
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
          @click="buttonPressed(button)">
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
