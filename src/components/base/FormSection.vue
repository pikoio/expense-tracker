<script setup>
import {computed, inject, ref} from "vue";
import { TRANSACTION_AMOUNT_LIMITS, TRANSACTION_TITLE_LIMITS } from "@/constants.js";
const emit = defineEmits(["add-transaction"])


const transactionTypes = inject("transactionTypes");
const incomeCategories = inject("incomeCategories");
const expenseCategories = inject("expenseCategories");

const defaultTransactionStructure = ref({
  title: "",
  amount: null,
  type: "Income",
  category: "",
})

const tempTransaction = ref({
  title: "",
  amount: null,
  type: "Income",
  category: "",
})

const isTitleValid = computed(() => {
  return tempTransaction.value.title.length >= TRANSACTION_TITLE_LIMITS.MIN_LENGTH &&
      tempTransaction.value.title.length <= TRANSACTION_TITLE_LIMITS.MAX_LENGTH
})

const isAmountValid = computed(() => {
  return tempTransaction.value.amount >= TRANSACTION_AMOUNT_LIMITS.MIN_AMOUNT &&
      tempTransaction.value.amount <= TRANSACTION_AMOUNT_LIMITS.MAX_AMOUNT
})

const isFormValid = computed(() => {
  return isTitleValid.value && isAmountValid.value &&
      tempTransaction.value.type !== "" && tempTransaction.value.category !== ""
})

const addTransaction = () => {
  if(!isFormValid.value) { return }
  const newTransaction = {
    id: crypto.randomUUID(),
    title: tempTransaction.value.title,
    amount: tempTransaction.value.amount,
    type: tempTransaction.value.type,
    category: tempTransaction.value.category,
  }
  emit("add-transaction", newTransaction)
  tempTransaction.value = {...defaultTransactionStructure.value}
}
</script>

<template>
  <div class="form-section">
    <p class="section-title">Add transaction</p>
    <form class="add-transaction-form" @submit.prevent="addTransaction">
      <label class="input-label">Title</label>
      <input v-model="tempTransaction.title" placeholder="Enter transaction title..." class="input-text" type="text">
      <label class="input-label">Amount</label>
      <input v-model="tempTransaction.amount" placeholder="Enter transaction amount..." class="input-text" type="number">
      <div class="transaction-type-row">
        <label v-for="type in transactionTypes">
          <input type="radio" :value="type" v-model="tempTransaction.type">
          {{ type }}
        </label>
      </div>
      <label class="input-label">Category</label>
      <select class="select-input" v-show="tempTransaction.type === 'Income'" v-model="tempTransaction.category">
        <option disabled value="">Please select category...</option>
        <option v-for="category in incomeCategories" :key="category.index" :value="category">{{ category }}</option>
      </select>
      <select class="select-input" v-show="tempTransaction.type === 'Expense'" v-model="tempTransaction.category">
        <option disabled value="">Please select category...</option>
        <option v-for="category in expenseCategories" :key="category.index" :value="category">{{ category }}</option>
      </select>
      <button :disabled="!isFormValid" type="submit" class="submit-btn">Add transaction</button>
    </form>
    {{tempTransaction}}
    {{isFormValid}}
  </div>
</template>

<style scoped>
.form-section {
  background-color: #d0e4e4;
  border-radius: 1.5rem;
  width: 30%;
  padding: 2rem;
  display: flex;
  flex-direction: column;

}

.form-section .section-title {
  font-size: 1.5rem;
  font-weight: 600;
  color: #434343;
  margin-bottom: 1rem;
}

.form-section .add-transaction-form {
  display: flex;
  flex-direction: column;
}

.form-section .input-label {
  color: #434344;
  font-size: 1rem;
}

.form-section .input-text {
  border: none;
  height: 2.5rem;
  margin-top: 0.3rem;
  margin-bottom: 1.5rem;
  border-radius: 0.5rem;
  padding-left: 1rem;
  padding-right: 1rem;
}

.form-section .transaction-type-row {
  height: 2.5rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-left: 5rem;
  padding-right: 5rem;
  margin-bottom: 1rem;
  font-size: 1.1rem;
  color: #434343;
}

.form-section .select-input {
  border: none;
  height: 2.5rem;
  border-radius: 0.5rem;
  margin-top: 0.5rem;
  padding-left: 0.5rem;
}

.form-section .submit-btn {
  margin-top: 5rem;
  height: 3.5rem;
  font-size: 1.1rem;
  color: #ededed;
  border: none;
  background-color: #6b8091;
  border-radius: 0.5rem;
}

</style>