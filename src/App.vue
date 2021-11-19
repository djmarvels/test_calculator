<template>
  <div id="app" class="calculator">
    <the-months v-model="Month" />

    <div class="calculator-row">
      <the-deposits v-model="Result" :month.sync="Month" />
      <the-status :result="Result" />
    </div>
  </div>
</template>

<script>
import TheMonths from './components/TheMonths';
import TheDeposits from './components/TheDeposits';
import TheStatus from './components/TheStatus';

export default {
  name: 'App',
  data: () => ({
    Month: 0,
    Result: 0,
  }),
  components: {
    TheMonths,
    TheDeposits,
    TheStatus
  },
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body {
  margin: 0;
  padding: 0;
}
.calculator {
  width: 100%;
  max-width: 1200px;
  margin-left: auto;
  margin-right: auto;

  &-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
}
</style>
