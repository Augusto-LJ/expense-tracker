<template>
    <Header />
    <div class="container">
      <Balance :total="total"/>
      <IncomeExpenses :income="+income" :expenses="+expenses"/>
      <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
      <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { useToast } from 'vue-toastification';

import { ref, computed } from 'vue';

const toast = useToast();

const transactions = ref([]);

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
          .filter(x => x.amount > 0)
          .reduce((acc, transaction) => {
            return acc + transaction.amount;
          }, 0)
          .toFixed(2);
});

const expenses = computed(() => {
  return transactions.value
          .filter(x => x.amount < 0)
          .reduce((acc, transaction) => {
            return acc + transaction.amount;
          }, 0)
          .toFixed(2);
});

const handleTransactionSubmitted = function(transactionData) {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount
  });

  toast.success('Transaction added!');
}

const generateUniqueId = function() {
  const maxIdObj = transactions.value[transactions.value.length - 1]
  return maxIdObj ? maxIdObj.id + 1 : 1;
}

const handleTransactionDeleted = function(id) {
  transactions.value = transactions.value.filter(x => x.id !== id);

  toast.success('Transaction deleted!');
}

</script>