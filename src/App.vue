<template>
  <Header />
  <div class="container">
    <Balance :total="total"/>
    <IncomeExpense :income="income" :expenses="expenses"/>
    <TransactionList :transactions="transactions"/>
    <AddTransaction />
  </div>
</template>

<script setup>
  import Header from './components/Header.vue'
  import Balance from './components/Balance.vue'
  import IncomeExpense from './components/IncomeExpense.vue'
  import TransactionList from './components/TransactionList.vue'
  import AddTransaction from './components/AddTransaction.vue'

  import { ref, computed  } from 'vue'

  const transactions = ref([
      { id: 1, text: 'Book', amount: -20 },
      { id: 2, text: 'Salary', amount: 500 },
      { id: 3, text: 'Tickets', amount: -208 },
      { id: 4, text: 'Camera', amount: 130 },
  ])

  //Get total
  const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0) 
  })

  //Get income
  const income = computed(() => {
    return transactions.value
      .filter((transaction) => transaction.amount > 0)
      .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0).toFixed(2)
  })

  //Get expenses
  const expenses = computed(() => {
    return transactions.value
      .filter((transaction) => transaction.amount < 0)
      .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0).toFixed(2)
  })
</script>