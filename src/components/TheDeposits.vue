<template>
  <div class="calculator-deposits">
    <div class="calculator-deposits-row">
      <div class="calculator-deposits-header">Сумма в работе</div>
      <ul class="calculator-deposits-list">
        <li v-for="(item, index) in depositsConfig" :key="index" class="calculator-deposits-item">
          <span v-html="item.name" />
          <input class="calculator-deposits-input"
                 :type="item.key === 'result' ? 'text' : 'number'"
                 v-model="item.value" @input="inputDeposit(item)"
                 :disabled="item.key === 'result'" />
        </li>
      </ul>
    </div>
    <div class="calculator-deposits-row">
      <div class="calculator-deposits-header">Доход в месяц</div>
      <ul class="calculator-deposits-list">
        <li v-for="objectKey in Object.keys(incomeConfig)" class="calculator-deposits-item" :key="objectKey">
          <span v-if="objectKey !== 'result'" v-html="`${depositItem(objectKey).div * 100} %`" class="calculator-deposits-small" />
          <span v-else class="calculator-deposits-small" />

          <span class="calculator-deposits-result" v-html="`${formatter.format(incomeConfig[objectKey])} $`" />
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TheDeposits',
  props: {
    month: {
      type: Number,
      required: true,
    },
  },
  data: () => ({
    depositsConfig: [
      { name: 'Личные депозиты', value: null, key: 'personal', div: 0.5, },
      { name: 'Линия 1', value: null, key: 'line.1', div: 0.5, },
      { name: 'Линия 2', value: null, key: 'line.2', div: 0.2, },
      { name: 'Линия 3', value: null, key: 'line.3', div: 0.15, },
      { name: 'Линия 4', value: null, key: 'line.4', div: 0.1, },
      { name: 'Линия 5', value: null, key: 'line.5', div: 0.05, },
      { name: 'Итого', value: null, key: 'result' },
    ],
    incomeConfig: {
      personal: 0,
      'line.1': 0,
      'line.2': 0,
      'line.3': 0,
      'line.4': 0,
      'line.5': 0,
      result: 0,
    },
    formatter: null,
  }),
  watch: {
    month() {
      this.calculateResult();
    },
  },
  created() {
    this.formatter = new Intl.NumberFormat('en-US', { currency: 'USD' });
  },
  methods: {
    inputDeposit(item) {
      if (Number(item.value) < 0) {
        item.value = null;
        return;
      } else if (item.key !== 'result') {
        this.incomeConfig[item.key] = Math.floor(Number(item.value) * Number(item.div));
      }
      this.calculateResult();
    },
    calculateResult() {
      // Считаем сумму в работе
      const values = this.depositsConfig.filter(item => (item.value && item.key !== 'result')).map(item => Number(item.value));
      const resultItem = this.depositsConfig.find(item => item.key === 'result');
      if (values.length && resultItem) resultItem.value = `${values.reduce((a, b) => (a + b))} $`;
      else resultItem.value = null;

      // Считаем доход в месяц
      const resultValues = Object.keys(this.incomeConfig).filter(key => (this.incomeConfig[key] && this.incomeConfig[key] > 0 && key !== 'result')).map(key => this.incomeConfig[key]);
      if (resultValues.length) {
        this.incomeConfig.result = resultValues.reduce((a, b) => (a + b));
        if (this.month > 0) this.incomeConfig.result *= this.month;
      } else this.incomeConfig.result = 0;

      this.$emit('input', this.incomeConfig.result);
    },
    depositItem(key) {
      return this.depositsConfig.find(item => item.key === key);
    },
  },
}
</script>

<style scoped lang="scss">
.calculator-deposits {
  margin-top: 20px;
  display: flex;
  align-items: flex-start;
  justify-content: flex-start;

  &-header {
    font-size: 18px;
    font-weight: 500;
    margin-bottom: 12px;
  }

  &-row {
    &:not(&:last-child) {
      margin-right: 40px;
    }
    &:last-child {
      .calculator-deposits-header {
        padding-left: 35px;
      }
    }
  }

  &-list {
    margin: 14px 0 0 0;
    padding: 0;
    list-style-type: none;
  }

  &-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    &:not(&:last-child) {
      margin-bottom: 12px;
    }

    span {
      font-size: 14px;
      font-weight: 400;
      line-height: 120%;
      margin-right: 12px;
      display: block;
      text-align: right;
      width: 100%;
    }
  }

  &-result {
    font-size: 16px !important;
    line-height: 18px !important;
    white-space: nowrap;
    text-align: left !important;
  }

  &-small {
    font-size: 10px !important;
    width: 30px !important;
    white-space: nowrap;
  }

  &-input {
    border: 0;
    padding: 0;
    margin: 0;
    font-size: 16px !important;
    line-height: 18px;
    background: none;
    -webkit-appearance: none;
    text-align: left;
    &:not(&[disabled]) {
      border-bottom: 1px solid #000000;
    }
    &:focus {
      outline: none;
    }
  }
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
input[type=number] {
  -moz-appearance: textfield;
}
</style>