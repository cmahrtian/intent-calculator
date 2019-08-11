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
      baseNumber: 0,
      modifyingNumber: 0,
      operandSelected: null,
      decimalAdded: false
    }
  },
  methods: {
    buttonPressed (button) {
      if (typeof button === 'number' || button === '.') {
        this.setDisplayedNumber(button)
      } else if (this.operands.indexOf(button) !== -1) {
        if (!this.baseNumber) {
          this.baseNumber = parseFloat(this.displayedNumber)
        } else {
          this.calculateBaseNumber(this.operandSelected) 
        }
        this.operandSelected = button
      } else {
        switch (button) {
          case '=':
            this.calculateBaseNumber(this.operandSelected)
            this.operandSelected = null
            this.decimalAdded = false
            break;
          case 'AC':
            this.displayedNumber = 0
            this.baseNumber = 0
            this.modifyingNumber = 0
            this.operandSelected = null
            break;
        }
      }
    },
    calculateBaseNumber (operand) {
      switch (operand) {
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
      this.decimalAdded = false
    },
    setDisplayedNumber (input) {
      if (this.displayedNumber === 0) {
        if (input === '.') {
          this.decimalAdded = true
        }
        this.displayedNumber += input
      } else {
        if (this.operandSelected) {
          if (this.modifyingNumber !== 0) {
            if (input === '.') {
              if (!this.decimalAdded) {
                this.modifyingNumber += input
                this.decimalAdded = true
              }
            } else {
              this.modifyingNumber = parseFloat(
                this.modifyingNumber.toString() + input.toString()
              )
            }
          } else {
            if (input === '.') {
              if (!this.decimalAdded) {
                this.modifyingNumber += input
                this.decimalAdded = true
              }
            } else {
              this.modifyingNumber += input
            }
          }
          this.displayedNumber = this.modifyingNumber
        } else {
          if (input === '.') {
            if (!this.decimalAdded) {
              this.displayedNumber += input
              this.decimalAdded = true
            }  
          } else {
            this.displayedNumber = parseFloat(
              this.displayedNumber.toString() + input.toString()
            )
          }
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
