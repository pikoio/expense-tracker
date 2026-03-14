<script setup>
import {computed, inject} from "vue";

const transactions = inject("transactions")
const totalIncome = computed(() => {
  const incomeTransactions = transactions.value.filter((t) => t.type === "Income")
  return incomeTransactions.reduce((acc, t) => {
    return acc + t.amount
  }, 0)
})
const totalExpense = computed(() => {
  const expenseTransactions = transactions.value.filter((t) => t.type === "Expense")
  return expenseTransactions.reduce((acc, t) => {
    return acc + t.amount
  }, 0)
})

const totalBalance = computed(() => {
  return totalIncome.value - totalExpense.value
})
</script>

<template>
  <div class="balance-section">
    <p class="section-title">My balance</p>
    <div class="total-balance">
      <p class="amount">{{ totalBalance }}</p>
      <p class="currency">zl</p>
    </div>
    <div class="calculations">
      <div class="income">
        <div class="indicator"></div>
        <p>+{{ totalIncome }}zl</p>
      </div>
      <div class="expense">
        <div class="indicator"></div>
        <p>-{{ totalExpense }}zl</p>
      </div>
    </div>

  </div>
</template>

<style scoped>
.balance-section{
  width: 45%;
  padding: 2rem;
  display: flex;
  flex-direction: column;
}
.balance-section .section-title {
  font-size: 1.5rem;
  font-weight: 600;
  color: #434343;
  margin-bottom: 1rem;
}
.balance-section .total-balance {

  height: 6rem;
  display: flex;
  align-items: baseline;
  color: #434343;
  gap: 0.2rem
}
.balance-section .total-balance .amount {
  font-size: 4rem;
}
.balance-section .total-balance .currency {
  font-size: 2.5rem;
}
.balance-section .calculations{

  display: flex;
  height: 3rem;
  font-size: 1.3rem;
  color: #434343;
}
.balance-section .calculations .income{

  width: 50%;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
.balance-section .calculations .income .indicator{
  height: 1rem;
  width: 1rem;
  border-radius: 50%;
  background-color: #88b58e;
}

.balance-section .calculations .expense{

  width: 50%;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
.balance-section .calculations .expense .indicator{
  height: 1rem;
  width: 1rem;
  border-radius: 50%;
  background-color: #bc7c7c;
}
</style>