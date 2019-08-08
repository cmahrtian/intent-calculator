<script>
export default {
  data () {
    return {
      buttonRows: [
        ['AC', '+/-', '%', '÷'],
        [7, 8, 9, 'X'],
        [4, 5, 6, '-'],
        [1, 2, 3, '+'],
        [0, '.', '=']
      ]
    }
  }
}
</script>

<template>
  <div class="calculator">
    <h1>Calculations go here</h1>
    <ul>
      <li v-for="(row, index) in buttonRows"
        :key="index">
        <span v-for="(button, index) in row"
          :key="index">
          <button>{{ button }}</button>
        </span>
      </li>
    </ul>
  </div>
</template>

<style lang="css" scoped>
  .calculator ul {
    list-style-type: none;
  }
</style>
