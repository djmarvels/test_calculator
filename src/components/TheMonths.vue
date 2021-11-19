<template>
  <div class="calculator-month">
    <button
        v-for="(item, index) in configMonth" :key="index"
        :class="[
            'calculator-month-item',
            { 'calculator-month-item--active': value === item.value }
        ]"
        v-html="item.name"
        @click="$emit('input', item.value)"
    />
  </div>
</template>

<script>
export default {
  name: 'TheMonths',
  props: {
    value: {
      type: Number,
      required: true,
    },
  },
  data: () => ({
    configMonth: [
      {
        name: 'За месяц',
        value: 1,
      },
      {
        name: 'За 3 месяца',
        value: 3,
      },
      {
        name: 'За 6 месяцев',
        value: 6,
      },
      {
        name: 'За год',
        value: 12,
      }
    ],
  }),
}
</script>

<style scoped lang="scss">
.calculator-month {
  display: flex;
  align-items: flex-start;
  justify-content: flex-start;
  margin-top: 20px;
  &-item {
    padding: 1rem;
    border: 1px solid #2f3640;
    border-radius: 5px;
    color: #000000;
    cursor: pointer;
    background-color: transparent;
    transition: all .3s 0s linear;
    &:not(&:last-child) {
      margin-right: 4px;
    }
    &--active {
      color: #fff;
      background-color: #d35400;
    }
  }
}
</style>
