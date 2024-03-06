<script setup>
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'

import { ref, computed, onMounted } from 'vue';

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
})

// Get total
const total = computed(() => {
  return transactions.value
    .reduce((total, transaction) => {
      return total + transaction.amount
    }, 0)
})

// Get income
const incomeTotal = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((total, transaction) => {
      return total + transaction.amount
    }, 0)
})


// Get expenses
const expensesTotal = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((total, transaction) => { return total + transaction.amount }, 0)
})

const handleTransactionSubmitted = (data) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: data.text,
    amount: data.amount,
  })

  saveTransactionsToLocalStorage();
}

const generateUniqueId = () => {
  return transactions.value.length + 1
}

function handleTransactionDeleted(id) {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id)
  saveTransactionsToLocalStorage();

}

// Saving to localstorage

const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

</script>

<template>
  <div>
    <Header></Header>
    <div class="container">
      <Balance :total="+total"></Balance>
      <IncomeExpenses :income="+incomeTotal" :expenses="+expensesTotal"></IncomeExpenses>
      <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
      <AddTransaction @transaction-submit="handleTransactionSubmitted"></AddTransaction>
    </div>
  </div>
</template>