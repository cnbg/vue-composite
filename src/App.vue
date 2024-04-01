<template>
  <Header/>
  <div class="container">
    <Balance :total="+total"/>
    <IncomeExpense :income="+income" :expense="+expense"/>
    <TransactionList :transactions="transactions" @onTransactionDelete="handleTransactionDelete" />
    <AddTransaction @onTransactionSubmit="handleTransactionSubmit"/>
  </div>
</template>

<script setup>
import Header from "@/components/expense/Header.vue"
import Balance from "@/components/expense/Balance.vue"
import IncomeExpense from "@/components/expense/IncomeExpense.vue"
import TransactionList from "@/components/expense/TransactionList.vue"
import AddTransaction from "@/components/expense/AddTransaction.vue"

import {useToast} from 'vue-toastification'
import {computed, ref, onMounted} from 'vue'

const transactions = ref([])

const toast = useToast()

onMounted(() => {
  transactions.value = JSON.parse(localStorage.getItem('transactions') ?? '[]')
})

// Get total
const total = computed(() => {
  return transactions
      .value
      .reduce((accumulator, transaction) => {
        return accumulator + transaction.amount
      }, 0)
      .toFixed(2)
})

// Get income
const income = computed(() => {
  return transactions
      .value
      .filter((transaction) => transaction.amount > 0)
      .reduce((accumulator, transaction) => {
        return accumulator + transaction.amount
      }, 0)
      .toFixed(2)
})

// Get expense
const expense = computed(() => {
  return transactions
      .value
      .filter((transaction) => transaction.amount < 0)
      .reduce((accumulator, transaction) => {
        return accumulator + transaction.amount
      }, 0)
      .toFixed(2)
})

const generateUniqueId = () => Math.floor(Math.random() * 1000000)

const handleTransactionSubmit = (data) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: data.text,
    amount: data.amount,
  })

  saveTransactionsToLocalStorage()

  toast.success('transaction added')
}

const handleTransactionDelete = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id)

  toast.success('transaction deleted')
}

const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>
