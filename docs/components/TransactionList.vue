<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li 
            v-for="transaction in transactions"
            :key="transaction.id"
            :class="transaction.amount < 0 ? 'minus' : 'plus'"
            @click="handleTransactionToUpdate(transaction)" 
        >
            {{ transaction.text }} <span>€{{ transaction.amount }}</span><button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>
    </ul>
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps({
    transactions: {
        type: Array,
        required: true
    }
})

const emit = defineEmits(['transactionDeleted', 'transactionToBeUpdated'], id)

const deleteTransaction = (id) => {
    emit('transactionDeleted', id)
}

const handleTransactionToUpdate = (transaction) => {
    emit('transactionToBeUpdated', transaction)
}
</script>