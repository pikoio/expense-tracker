<script setup>
import {inject} from "vue";

const transaction = inject('transactions')
</script>

<template>
  <div class="list-section">
    {{transaction}}
  </div>
</template>

<style scoped>
.list-section{
  background-color: #d0e4e4;
  border-radius: 1.5rem;
  width: 60%;
}
</style>