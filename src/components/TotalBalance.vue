<template>
  <div :class="totalClass" class="total-value">Balance: {{ total }}</div>
</template>
<script>

export default {
  name: 'TotalBalance',
  props: {
    total: {
      type: Number,
      default: 0
    }
  },
  computed: {
    totalClass() {
      if (this.total > 0) {
        return 'total-positive';
      } else if (this.total < 0) {
        return 'total-negative';
      } else {
        return 'total-zero';
      }
    }
  },
}

</script>
<style scoped>
  .total-value {
    font-size: 26px;
    text-transform: uppercase;
    padding: 20px;
    text-align: center;
  }
  .total-positive {
    color: green;
  }
  .total-negative {
    color: red;
  }
  .total-zero {
    color: black;
  }
</style>