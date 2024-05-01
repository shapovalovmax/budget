<template>
  <div class="budget-list-wrap">
    <el-card :header="header">
      <template v-if="!isEmpty">
        <div class="filter-buttons-wrapper">
          <el-button @click="onFilterIn" type="success" value="onlyIN">Only IN</el-button>
          <el-button @click="onFilterOut" type="danger" value="onlyOUT">Only OUT</el-button>
          <el-button @click="onFilterAll" type="primary" value="showALL">Show ALL</el-button>
        </div>
        <BudgetListItem v-for="(item, prop) in list" :key="prop" :item="item" @deleteItem="openDialog"/>
      </template>
      <ElAlert v-else type="info" :title="emptyTitle" :closable="false"/>
    </el-card>
    <el-dialog title="Warning" :visible="dialogVisible" @close="dialogVisible = false" width="30%" center>
      <div style="text-align: center;">
        <span>Are you sure you want to delete this item?</span>
      </div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">Cancel</el-button>
        <el-button type="primary" @click="confirmDelete">Confirm</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import BudgetListItem from "@/components/BudgetListItem";

export default {
  name: "BudgetList",
  components: {
    BudgetListItem
  },
  props: {
    list: {
      type: Object,
      default: () => ({})
    }
  },
  data() {
    return {
      header: 'Budget List',
      emptyTitle: 'Empty List',
      dialogVisible: false,
      itemToDelete: null
    };
  },
  computed: {
    isEmpty() {
      return !Object.keys(this.list).length;
    }
  },
  methods: {
    openDialog(id) {
      this.itemToDelete = id;
      this.dialogVisible = true;
    },
    confirmDelete() {
      this.$emit('deleteItem', this.itemToDelete);
      this.dialogVisible = false;
    },
    onFilterIn() {
        // При кліку на кнопку "Only IN", приховати всі елементи списку, які не мають класу "income-item"
        const listItems = document.querySelectorAll('.list-item');
        listItems.forEach(item => {
          if (!item.classList.contains('income-item')) {
            item.style.display = 'none';
          } else {
            item.style.display = 'flex';
          }
        });
    },
    onFilterOut() {
      const listItems = document.querySelectorAll('.list-item');
      listItems.forEach(item => {
        if (!item.classList.contains('outcome-item')) {
          item.style.display = 'none';
        } else {
          item.style.display = 'flex';
        }
      });
    },
    onFilterAll() {
      const listItems = document.querySelectorAll('.list-item');
      listItems.forEach(item => {
        item.style.display = 'flex';
      });
    }
  }
}

</script>

<style scoped>
.filter-buttons-wrapper {
  margin-top: 10px;
  margin-bottom: 20px;
}
.budget-list-wrap {
  max-width: 500px;
  margin: auto;
}

.list-item {
  display: flex;
  align-items: center;
  padding: 10px 15px;
}

.budget-value {
  font-weight: bold;
  margin-left: auto;
  margin-right: 20px;
}
</style>