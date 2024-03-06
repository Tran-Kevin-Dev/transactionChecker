<script setup>
import { ref, transformVNodeArgs } from 'vue';

const text = ref('');
const amount = ref('');
const formError = ref({
    borderRed: false
})

const emit = defineEmits(['transactionSubmit']);

const onSubmit = () => {
    if (!text.value || !amount.value) {
        alert('You forgot to fill in the fields brother')
        formError.value.borderRed = true
    }
    formError.value.borderRed = false
    const transactionData = {
        text: text.value,
        amount: amount.value
    }
    emit('transactionSubmit' , transactionData)
}

</script>

<template>
    <div>
        <h3>Add new transaction</h3>
        <form id="form" @submit.prevent="onSubmit">
            <div class="form-control">
                <label for="text">Text</label>
                <input :class="formError" v-model="text" type="text" id="text" placeholder="Enter text..." />
            </div>
            <div class="form-control">
                <label for="amount">Amount <br />
                    (negative - expense, positive - income)</label>
                <input :class="formError" v-model="amount" type="number" id="amount" placeholder="Enter amount..." />
            </div>
            <button class="btn">Add transaction</button>
        </form>
    </div>
</template>

<style scoped>
.borderRed {
    border: solid 2px red;
}
</style>