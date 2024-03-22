<template>   
  <div>
    <h3>{{ buttonText }}</h3>
    <form id="form" @submit.prevent="onSubmit">
      <div class="form-control">
        <label for="text">Text</label>
        <input type="text" id="text" v-model="text" placeholder="Enter text..." />
      </div>
      <div class="form-control">
        <label for="amount">Amount (negative - expense, positive - income)</label>
        <input type="number" id="amount" v-model="amount" placeholder="Enter amount..." />
      </div>
      <button class="btn">{{ buttonText }}</button>
    </form>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import { useToast } from "vue-toastification";

const props = defineProps({
  transaction: {
    type: Object,
    required: true
  }
})

const text = ref('')
const amount = ref('')
const emit = defineEmits(['transactionSubmitted', 'transactionUpdated'])

watch(() => props.transaction, (newValue) => {
  if (newValue) {
    text.value = newValue.text
    amount.value = newValue.amount
    buttonText.value = 'Update Transaction'
  }
})

const buttonText = ref(props.transaction ? 'Update Transaction' : 'Add Transaction')

const onSubmit = () => {
  const toast = useToast();

  if (!text.value || !amount.value) {
    toast.error('Both fields must be filled!')
    return
  }

  const transactionData = {
    text: text.value,
    amount: amount.value
  }

  if (props.transaction) {
    emit('transactionUpdated', transactionData)
  } else {
    emit('transactionSubmitted', transactionData)
  }

  text.value = ''
  amount.value = ''
}
</script>


