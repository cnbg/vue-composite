<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="submit">
    <div class="form-control">
      <label for="text">Text</label>
      <input v-model="text" type="text" id="text" placeholder="Enter text..."/>
    </div>
    <div class="form-control">
      <label for="amount">Amount <br/>(negative - expense, positive - income)</label>
      <input v-model="amount" type="number" id="amount" placeholder="Enter amount..."/>
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import {ref} from 'vue'
import {useToast} from 'vue-toastification'

const text = ref('')
const amount = ref('')

const emit = defineEmits(['onTransactionSubmit'])

const toast = useToast()

const submit = () => {
  if (!text.value || !amount.value) {
    toast.error("fill both fields")
    return
  }

  const transactionData = {
    text: text.value,
    amount: amount.value,
  }

  emit('onTransactionSubmit', transactionData)

  text.value = ''
  amount.value = ''
}
</script>
