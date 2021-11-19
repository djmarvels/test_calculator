<template>
  <div class="calculator-status">
    <div class="calculator-status-col" v-for="status in statusConfig" :key="status.value.min">
      <div :class="[
          'calculator-status-box',
          { 'calculator-status-box--active': result >= status.value.min && result < status.value.max }
      ]" v-html="status.name" />
      <span class="calculator-status-small" v-html="`${formatter.format(status.value.min)} $`" />
    </div>
  </div>
</template>

<script>
export default {
  name: "TheStatus",
  props: {
    result: {
      type: Number,
      required: true,
    }
  },
  data: () => ({
    statusConfig: [
      {
        name: 'Manager',
        value: {
          min: 100000,
          max: 200000,
        },
      },
      {
        name: 'Director',
        value: {
          min: 200000,
          max: 500000,
        },
      },
      {
        name: 'Silver Director',
        value: {
          min: 500000,
          max: 1000000,
        },
      },
      {
        name: 'Gold Director',
        value: {
          min: 1000000,
          max: 5000000,
        },
      },
      {
        name: 'Platinum Director',
        value: {
          min: 5000000,
          max: 10000000,
        },
      },
      {
        name: 'Global Director',
        value: {
          min: 10000000,
          max: Infinity,
        },
      }
    ],
    formatter: null,
  }),
  created() {
    this.formatter = new Intl.NumberFormat('en-US', { currency: 'USD' });
  },
}
</script>

<style scoped lang="scss">
.calculator-status {
  display: flex;
  align-items: flex-start;
  justify-content: flex-start;
  flex-wrap: wrap;
  width: 50%;

  &-col {
    flex-basis: calc(100% / 3);
    display: flex;
    align-items: center;
    justify-content: flex-start;
    flex-direction: column;

    &:nth-last-child(1), &:nth-last-child(2), &:nth-last-child(3) {
      margin-top: 16px;
    }
  }

  &-box {
    border: 1px solid #000000;
    font-size: 16px;
    line-height: 16px;
    padding: 20px;
    border-radius: 5px;
    transition: all .3s 0s linear;

    &--active {
      color: #ffffff;
      background-color: #d35400;
    }
  }

  &-small {
    font-size: 12px;
    text-align: center;
    display: block;
    width: 100%;
    margin-top: 16px;
  }
}
</style>