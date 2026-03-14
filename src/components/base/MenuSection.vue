<script setup>
import {inject} from "vue";
import ListSection from "@/components/base/ListSection.vue";
import BalanceSection from "@/components/base/BalanceSection.vue";

const transaction = inject('transactions')

const emit = defineEmits(["delete-transaction"]);
const deleteTransaction = (transactionId) => {
  emit("delete-transaction", transactionId);
}
</script>

<template>
  <div class="menu-section">
    <ListSection @delete-transaction = deleteTransaction />
    <BalanceSection/>
  </div>
</template>

<style scoped>
.menu-section{
  background-color: #d0e4e4;
  border-radius: 1.5rem;
  width: 60%;
  display: flex;
}
</style>