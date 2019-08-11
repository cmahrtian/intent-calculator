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
            if (this.baseNumber !== 0) {
              this.calculateBaseNumber(this.operandSelected)
              this.operandSelected = null
              this.decimalAdded = false
            }
            break
          case 'AC':
            this.displayedNumber = 0
            this.baseNumber = 0
            this.modifyingNumber = 0
            this.operandSelected = null
            break
          case '+/-':
            if (this.baseNumber === 0 && this.modifyingNumber === 0) {
              this.displayedNumber = this.displayedNumber * -1
            } else if (this.baseNumber !== 0 && this.modifyingNumber === 0) {
              this.baseNumber = this.baseNumber * -1
              this.displayedNumber = this.baseNumber
            } else {
              this.modifyingNumber = this.modifyingNumber * -1
              this.displayedNumber = this.modifyingNumber
            }
            break
          case '%':
            if (this.baseNumber === 0 && this.modifyingNumber === 0) {
              this.displayedNumber = this.displayedNumber / 100
            } else if (this.baseNumber !== 0 && this.modifyingNumber === 0) {
              this.baseNumber = this.baseNumber / 100
              this.displayedNumber = this.baseNumber
            } else {
              this.modifyingNumber = this.modifyingNumber / 100
              this.displayedNumber = this.modifyingNumber
            }
            break
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
      this.displayedNumber = this.exceedsMaxNumberLength(this.baseNumber)
        ? this.baseNumber.toExponential(2)
        : this.baseNumber
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
    },
    exceedsMaxNumberLength (number) {
      return number.toString().length > 9
    }
  }
}
</script>

<template>
  <div class="calculator">
    <h1 :style="{ 'font-size': this.exceedsMaxNumberLength(this.displayedNumber) 
      ? '1em' : '2em' }">
      {{ displayedNumber }}
    </h1>
    <ul>
      <li v-for="(row, index) in buttonRows"
        :key="index">
        <span v-for="(button, index) in row"
          :key="index"
          @click="buttonPressed(button)">
          <button :data-label="button">{{ button }}</button>
        </span>
      </li>
    </ul>
  </div>
</template>

<style lang="css" scoped>
  .calculator {
    width: 12em;
    margin: 0 auto;
    border: 1px solid black;
    border-radius: 0.25em;
    background: #1C1C1C;
  }

  h1 {
    text-align: right;
    margin-block-end: -0.25em;
    padding-right: 0.3em;
    line-height: 24px;
    color: white;
  }
  
  ul {
    list-style-type: none;
    padding-left: 0;
    margin-block-end: 0;
  }

  button {
    font-size: 1.5em;
    cursor: pointer;
    width: 2em;
    color: white;
    background: #D4D4D2;
    border-color: #1C1C1C;
  }

  button[data-label="0"] {
    width: 4em;
    border-radius: 0 0 0 0.25em;
  }

  button[data-label="="] {
    border-radius: 0 0 0.25em 0;
  }
  
  button[data-label="AC"], 
  button[data-label="+/-"], 
  button[data-label="%"] {
    background: #505050;
  }

  button[data-label="÷"],
  button[data-label="x"],
  button[data-label="-"],
  button[data-label="+"],
  button[data-label="="] {
    background: #FF9500;
  }
</style>
