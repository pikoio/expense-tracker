<script setup>
import TransactionItem from "@/components/base/TransactionItem.vue";
import {computed, inject, ref, watch} from "vue";
const transactions = inject("transactions");
const incomeCategories = inject("incomeCategories");
const expenseCategories = inject("expenseCategories");
const transactionTypes = inject("transactionTypes");

const emit = defineEmits(["delete-transaction"]);

const filterType = ref("All")
const filterCategory = ref("All")

const categoriesMap = computed(() => {
  if(filterType.value === "Income") return incomeCategories.value
  if(filterType.value === "Expense") return expenseCategories.value
  return []
})
const currentCategories = computed(() => {
  return categoriesMap
})

const filteredTransactionsByType = computed(() => {
  if(filterType.value === "All"){ return transactions.value }
  return transactions.value.filter(t => t.type === filterType.value)
})
const filteredTransactionsByCategory = computed(() => {
  if(filterCategory.value === "All"){ return filteredTransactionsByType.value}
  return filteredTransactionsByType.value.filter(t => t.category === filterCategory.value)
})

const deleteTransaction = (transactionId) => {
  emit("delete-transaction", transactionId)
}

watch(filterType,() =>{
  filterCategory.value = "All"
})

</script>

<template>
  <div class="list-section">
    <div class="row-1">
      <p class="section-title">Transactions</p>

      <select v-if="filterType !== 'All'" v-model="filterCategory" class="filter-select">
        <option value="All">All</option>
        <option v-for="category in currentCategories" :key="category" :value="category">{{ category }}</option>
      </select>

      <select class="filter-select" v-model="filterType">
        <option value="All">All</option>
        <option v-for="type in transactionTypes" :value="type" :key="type">{{ type }}</option>
      </select>
    </div>
    <div class="transactions-wrapper">
      <TransactionItem
          v-for="transaction in filteredTransactionsByCategory"
          :key="transaction.id" :transaction="transaction"
          @delete-transaction="deleteTransaction(transaction.id)"/>
      <p v-if="filteredTransactionsByCategory.length === 0" class="no-transactions-err">There are no transactions...</p>
    </div>
  </div>
</template>

<style scoped>
.list-section{
  width: 55%;
  padding: 2rem;
  display: flex;
  flex-direction: column;
}
.list-section .row-1 .section-title {
  font-size: 1.5rem;
  font-weight: 600;
  color: #434343;
  margin-bottom: 1rem;
}
.list-section .row-1{
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
  align-items: center;
}
.list-section .row-1 .filter-select{
  height: 2rem;
  border: none;
  padding-left: 0.5rem;
  border-radius: 0.5rem;
  color: #434343;
}
.list-section .transactions-wrapper{
  flex: 1;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  padding-bottom: 1rem;
}
.transactions-wrapper .no-transactions-err{
  font-size: 1.2rem;
}
</style>