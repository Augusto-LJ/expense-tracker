<template>
    <Header />
    <div class="container">
      <Balance :total="total"/>
      <IncomeExpenses :income="income" :expenses="expenses"/>
      <TransactionList :transactions="transactions"/>
      <AddTransaction />
    </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { ref, computed } from 'vue';

const transactions = ref([
  {id: 1, text: "Flower", amount: -19.99},
  {id: 2, text: "Salary", amount: 2000.00},
  {id: 3, text: "Book", amount: -15},
  {id: 4, text: "Camera", amount: -299.99},
]);

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return Number(transactions.value
          .filter(x => x.amount > 0)
          .reduce((acc, transaction) => {
            return acc + transaction.amount;
          }, 0)
          .toFixed(2));
})

const expenses = computed(() => {
  return Number(transactions.value
          .filter(x => x.amount < 0)
          .reduce((acc, transaction) => {
            return acc + transaction.amount;
          }, 0)
          .toFixed(2));
})

</script>