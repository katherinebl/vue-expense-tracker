<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li 
            v-for="transaction in transactions"
            :key="transaction.id"
            :class="transaction.amount < 0 ? 'minus' : 'plus'"
        >
            {{ transaction.text }} <span>€{{ transaction.amount }}</span><button class="delete-btn">x</button>
        </li>
    </ul>
</template>

<script>
    export default {
        data: function() {
            return {
                transactions: [
                    { id: 1, text: 'Book', amount: -20 },
                    { id: 2, text: 'Salary', amount: 500 },
                    { id: 3, text: 'Tickets', amount: -208 },
                    { id: 4, text: 'Camera', amount: 130 },
                ]
            }
        }
    }
</script>