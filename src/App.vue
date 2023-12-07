<script setup>
  import { ref, computed, onMounted } from 'vue';
  import { useToast } from 'vue-toastification';
  import Header from '@/components/Header.vue';
  import Balance from '@/components/Balance.vue';
  import IncomeExpense from '@/components/IncomeExpense.vue';
  import TransactionList from '@/components/TransactionList.vue';
  import TransactionForm from '@/components/TransactionForm.vue';

  const transactions = ref([]);

  onMounted(() => {
    transactions.value = getTransactions();
  });

  const toast = useToast();

  const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
      const amount = transaction.type === 'expense' ? (-1) * transaction.amount : transaction.amount;

      return acc + amount;
    }, 0)
        .toFixed(2);
  });

  const totalIncome = computed(() => {
    return transactions.value
        .filter(transaction => transaction.type === 'income')
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2);
  });

  const totalExpenses = computed(() => {
    return transactions.value
        .filter(transaction => transaction.type === 'expense')
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2);
  });

  const handleTransactionAdded = (transactionData) => {
    transactions.value.push({
      id         : generateUniqueId(),
      description: transactionData.description,
      amount     : transactionData.amount,
      type       : transactionData.type,
    });

    saveTransactions();

    toast.success('Transaction added');
  }

  const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000);
  }

  const handleTransactionDeleted = (transactionId) => {
    transactions.value = transactions.value.filter(transaction => transaction.id !== transactionId);

    saveTransactions();

    toast.success('Transaction deleted');
  }

  const getTransactions = () => {
    return JSON.parse(localStorage.getItem('transactions')) ?? [];
  }

  const saveTransactions = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
  }
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />

    <IncomeExpense
        :income="+totalIncome"
        :expenses="+totalExpenses"
    />

    <TransactionForm @transaction-added="handleTransactionAdded" />

    <TransactionList
        @transaction-deleted="handleTransactionDeleted"
        :transactions="transactions"
    />
  </div>
</template>

<style scoped>

</style>