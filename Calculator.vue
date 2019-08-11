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
      symbols: ['x', '-', '+', '÷'],
      // number that's showing at the top of the calculator
      displayedNumber: 0,
      // first number in the mathematical operation 
      baseNumber: 0,
      // second number in the mathematical operation
      modifyingNumber: 0,
      symbolSelected: null,
      // whether a decimal has been added to displayedNumber
      // (can only happen once)
      decimalAdded: false
    }
  },
  methods: {
    buttonPressed (button) {
      // pressing one of the numbers or the decimal point
      // updates the value of the displayed number
      if (typeof button === 'number' || button === '.') {
        this.setDisplayedNumber(button)
      // pressing one of the symbols updates the values
      // of baseNumber and/or modifyingNumber
      } else if (this.symbols.indexOf(button) !== -1) {
        // new value set for baseNumber if not previously created
        if (!this.baseNumber) {
          this.baseNumber = parseFloat(this.displayedNumber)
        // new value for baseNumber calculated based on
        // symbol selected
        } else {
          this.calculateBaseNumber(this.symbolSelected) 
        }
        this.symbolSelected = button
      } else {
        switch (button) {
          // displays value of baseNumber if equal sign is clicked
          // only if baseNumber has been set
          case '=':
            if (this.baseNumber !== 0) {
              this.calculateBaseNumber(this.symbolSelected)
              this.symbolSelected = null
              this.decimalAdded = false
            }
            break
          // clears everything
          case 'AC':
            this.displayedNumber = 0
            this.baseNumber = 0
            this.modifyingNumber = 0
            this.symbolSelected = null
            break
          // determines what number should be toggled positive/negative
          // if clicked, multiples pertinent number by -1  
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
          // determines what number should be converted to a percentage
          // if clicked, divides pertinent number by 100
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
    calculateBaseNumber (symbol) {
      // baseNumber calculated based on value of symbol (i.e. symbolSelected)
      switch (symbol) {
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
      // sets value of displayedNumber to either baseNumber or an exponential
      // of baseNumber if it's longer than nine numbers
      this.displayedNumber = this.exceedsMaxNumberLength(this.baseNumber)
        ? this.baseNumber.toExponential(2)
        : this.baseNumber
      // value of modifyingNumber is re-set to 0, decimalAdded reverts to false
      this.modifyingNumber = 0
      this.decimalAdded = false
    },
    setDisplayedNumber (input) {
      // if displayedNumber is 0, this updates it
      // if a decimal is added, decimalAdded toggles to true
      // and can't be called anymore
      if (this.displayedNumber === 0) {
        if (input === '.') {
          this.decimalAdded = true
        }
        this.displayedNumber += input
      } else {
        // if an symbol has been selected, edits are made to modifyingNumber
        if (this.symbolSelected) {
          // if modifyingNumber already exists, the existing string is appended
          // with either a new number or a decimal point
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
          // if modifyingNumber doesn't exist yet, a new string is created
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
          // the value of displayedNumber
          // is set to the value of modifyingNumber
          this.displayedNumber = this.modifyingNumber
        } else {
          // if an symbol hasn't been selected, edits are made to displayedNumber
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
