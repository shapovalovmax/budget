<template>
  <div class="list-item" :class="{ 'income-item': item.type === 'INCOME', 'outcome-item': item.type === 'OUTCOME' }">
    <span class="budget-comment">{{ item.comment }}</span>
    <span class="budget-value" :style="{ color: item.type === 'INCOME' ? 'green' : 'red' }">{{ item.value }}</span>
    <i class="el-icon-top" v-if="item.type === 'INCOME'"></i>
    <i class="el-icon-bottom" v-else></i>
    <ElButton type="danger" size="mini" @click="deleteItem(item.id)">Delete</ElButton>
  </div>
</template>

<script>
export default {
  name: 'ListItem',
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  methods: {
    deleteItem(id) {
      this.$emit('deleteItem', id);
    }
  }
}
</script>

<style scoped>
.list-item {
  display: flex;
  align-items: center;
  padding: 10px 15px;
}
.budget-value {
  font-weight: bold;
  margin-left: auto;
}
i {
  margin-right: 20px;
}
</style>
