<script setup>
  import { ref, defineEmits } from 'vue';
  import { useToast } from 'vue-toastification';

  const description = ref('');
  const amount      = ref('');
  const type        = ref('income');

  const toast = useToast();

  const emit = defineEmits(['transactionAdded'])

  const addTransaction = () => {
    if (! description.value || ! amount.value) {
      toast.error('Both fields are required');
      return;
    }

    emit('transactionAdded', {
      description : description.value,
      amount      : parseFloat(amount.value),
      type        : type.value
    });

    description.value = '';
    amount.value      = '';
    type.value        = 'income';
  }
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="addTransaction">
    <div class="form-control">
      <label for="description">Short description</label>
      <input
          v-model="description"
          type="text"
          id="description"
          placeholder="Enter description..."
      />
    </div>
    <div class="form-control">
      <label for="amount">
        Amount
      </label>
      <input
          v-model="amount"
          type="text"
          id="amount"
          placeholder="Enter amount..."
          oninput="this.value = this.value.replace(/[^\d.]/g, ''); this.value = this.value.replace(/(\..*)\./g, '$1');"
      />
    </div>
    <div class="form-control">
      <label for="type">
        Type
      </label>
      <select id="type" v-model="type">
        <option value="income">Income</option>
        <option value="expense">Expense</option>
      </select>
    </div>
    <button type="submit" class="btn">Add transaction</button>
  </form>
</template>

<style scoped>

</style>