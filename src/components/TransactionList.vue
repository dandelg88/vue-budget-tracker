<script setup>
  import { defineProps, defineEmits } from 'vue';

  const props = defineProps(
    {
      transactions: {
        type: Array,
        required: true
      }
    }
  );

  const emit = defineEmits(['transactionDeleted']);

  const deleteTransaction = (transactionId) => {
    emit('transactionDeleted', transactionId);
  }
</script>

<template>
  <div v-if="transactions.length">
    <h3>History</h3>
    <ul id="list" class="list">
      <li v-for="transaction in transactions"
          :key="transaction.id"
          :class="transaction.type === 'expense' ? 'minus' : 'plus'"
      >
        {{ transaction.description }} <span>${{ transaction.amount }}</span>
        <button
            @click="deleteTransaction(transaction.id)"
            class="delete-btn"
        >x</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>

</style>