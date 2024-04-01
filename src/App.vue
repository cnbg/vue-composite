<template>
  <Header/>
  <div class="container">
    <Balance :total="+total"/>
    <IncomeExpense :income="+income" :expense="+expense"/>
    <TransactionList :transactions="transactions"/>
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
import {computed, ref} from 'vue'

const transactions = ref([
  {id: 1, text: 'Flower', amount: -20.42},
  {id: 2, text: 'Salary', amount: 300.34},
  {id: 3, text: 'Book', amount: -10.42},
  {id: 4, text: 'Camera', amount: 150.34},
])

const toast = useToast()

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

const handleTransactionSubmit = (data) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: data.text,
    amount: data.amount,
  })

  toast.success('transaction added')
}

const generateUniqueId = () => Math.floor(Math.random() * 1000000)
</script>
