<template>
  <Header />
  <div class="container">
    <Balance :total="+total"/>
    <IncomeExpense :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>
</template>

<script setup>
  import Header from './components/Header.vue'
  import Balance from './components/Balance.vue'
  import IncomeExpense from './components/IncomeExpense.vue'
  import TransactionList from './components/TransactionList.vue'
  import AddTransaction from './components/AddTransaction.vue'

  import { ref, computed, onMounted  } from 'vue'

  import { useToast } from "vue-toastification";

  const toast = useToast()

  const transactions = ref([])

  onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem("transactions"))

    if(savedTransactions) {
      transactions.value = savedTransactions
    }
  })

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

  //Add Transaction
  const handleTransactionSubmitted = (transactionData) => {    
    transactions.value.push({
      id: generateRandomId(),
      text: transactionData.text,
      amount: parseInt(transactionData.amount)
    })

    saveTransactionsInLS()

    toast.success('Trasaction added!')
  }

  const generateRandomId = () => {
    return Math.floor(Math.random() * 1000000)
  }

  //Delete Transaction
  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter(transaction => transaction.id !== id)

    saveTransactionsInLS()

    toast.success('Trasaction deleted!')
  }

  //Save in LocalStorage
  const saveTransactionsInLS = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
  }
</script>