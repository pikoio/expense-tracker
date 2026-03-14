<script setup>
import FormSection from "@/components/base/FormSection.vue";
import MenuSection from "@/components/base/MenuSection.vue";
import {provide, ref, watch} from "vue";

const transactions = ref([
  {
    id: "16dc393d-98w1-4e2e-bhe4-f8f7946fb66e",
    title: "5 apples from grocery",
    amount: 234,
    category: 'Grocery',
    type: "Expense",
    date: new Date().toISOString(),
  },
  {
    id: "78dc343d-28w1-4e2e-bhr4-f8f7946fb34e",
    title: "Salary from work",
    amount: 2000,
    category: 'Work',
    type: "Income",
    date: new Date().toISOString(),
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
  if (!transactionId) return
  transactions.value = transactions.value.filter((t) => t.id !== transactionId)
}

// watch(transactions, (newVal) => {
//   localStorage.setItem("transactions", JSON.stringify(newVal))
// }, { deep: true })
//
// const saved = localStorage.getItem("transactions")
//
// if (saved) transactions.value = JSON.parse(saved)


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
