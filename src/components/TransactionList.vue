<script setup>

const props = defineProps({
    transactions: {
        type: Array,
        required: true,
    }
})

const emit = defineEmits(['transactionDeleted'])

function deleteTransaction(transactionId) {
    emit('transactionDeleted', transactionId)
}

</script>

<template>
    <div>
        <h3>History</h3>
        <ul id="list" class="list">
            <li v-for="transaction in transactions" :key="transaction.id"
                :class="transaction.amount < 0 ? 'minus' : 'plus'">
                {{ transaction.text }} <span>{{ transaction.amount }} EUR</span>
                <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
            </li>
        </ul>
    </div>
</template>