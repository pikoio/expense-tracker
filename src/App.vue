<script setup>

import FormSection from "@/components/base/FormSection.vue";
import MenuSection from "@/components/base/MenuSection.vue";
import {provide, ref} from "vue";

const transactions = ref([
  {
    id: 1,
    title: "5 apples from grocery",
    amount: 234,
    category: 'Grocery',
    type: "Expense"
  },
  {
    id: 2,
    title: "Salary from work",
    amount: 2000,
    category: 'Work',
    type: "Income"
  }
])

const transactionTypes = ref(["Income", "Expense"])
const incomeCategories = ref(["Work", "Family", "Other income"])
const expenseCategories = ref(["Grocery", "Hobby", "Other expense"])

provide("transactions", transactions)
provide("transactionTypes", transactionTypes)
provide("incomeCategories", incomeCategories)
provide("expenseCategories", expenseCategories)

const addTransaction = (newTransaction) => {
  transactions.value.push(newTransaction)
}
const deleteTransaction = (transactionId) => {
  if(!transactionId) return
  transactions.value = transactions.value.filter((t) => t.id !== transactionId)
}
</script>

<template>

  <div class="app-container">
    <FormSection @add-transaction="addTransaction"/>
    <MenuSection @delete-transaction="deleteTransaction"/>
  </div>

</template>

<style scoped>
.app-container {
  border: 1px solid red;
  height: 100vh;
  padding: 3rem 10rem;
  display: flex;
  justify-content: space-between;
}
</style>
