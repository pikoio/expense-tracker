<script setup>
  import {computed} from "vue";

  const props = defineProps(["transaction"])
  const emit = defineEmits(["delete-transaction"])

  const formattedDate = computed(() => {
    const date = new Date(props.transaction.date)
    return date.toLocaleDateString() + " "
        + date.getUTCHours() + ":"
        + String(date.getUTCMinutes()).padStart(2, "0")
  })
</script>

<template>
  <div class="transaction-item">
    <div class="info">
      <div class="clm-1">
        <div class="row-1">
          <p>{{ transaction.title }}</p>
        </div>
        <div class="row-2">
          <p>{{ transaction.category }}</p>
          <p>{{ formattedDate }}</p>
        </div>
      </div>
      <div class="clm-2">
        <p v-if="transaction.type === 'Income'">+{{ transaction.amount }}zl</p>
        <p v-if="transaction.type === 'Expense'">-{{ transaction.amount }}zl</p>
      </div>
      <div class="clm-3">
        <button @click="$emit('delete-transaction')" class="delete-btn">X</button>
      </div>
    </div>
    <div class="type-indicator" :class="transaction.type?.toLowerCase()"></div>
  </div>
</template>

<style scoped>
.transaction-item{
  height: 5.5rem;
  background-color: #fdfdfd;
  border-radius: 0.5rem;
  display: flex;
  flex-shrink: 0;
}
.transaction-item .info{
  width: 97%;
  display: flex;
  padding: 0.8rem 0.8rem 0.8rem 1.5rem;
}
.transaction-item .info .clm-1{
  width: 65%;
  display: flex;
  flex-direction: column;
}
.transaction-item .info .clm-1 .row-1{
  height: 60%;
  display: flex;
  align-items: center;
  font-size: 1.1rem;

}
.transaction-item .info .clm-1 .row-2{
  display: flex;
  align-items: center;
  font-size: 0.9rem;
  color: #434343;
  height: 40%;
  gap: 1rem;
}
.transaction-item .info .clm-2{
  width: 25%;
  display: flex;
  align-items: center;
  justify-content: end;
  font-size: 1.2rem;
}
.transaction-item .info .clm-3{

  width: 10%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.transaction-item .info .clm-3 .delete-btn{
  border: none;
  background-color: #bd7d7d;
  width: 1.5rem;
  height: 1.5rem;
  border-radius: 0.3rem;
  color: #ffffff;
  cursor: pointer;
}
.transaction-item .info .clm-3 .delete-btn:hover{
  background-color: #a16161;
}
.transaction-item .type-indicator{
  width: 3%;
  border-radius: 0 0.5rem 0.5rem 0
}
.type-indicator.income{
  background-color: #89b68f;
}
.type-indicator.expense{
  background-color: #bd7d7d;
}
</style>