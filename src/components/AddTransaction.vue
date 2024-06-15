<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div id="form-control">
            <label for="text">Remarks</label>
            <input type="text" id="text" v-model="text" placeholder="please enter remarks...">
        </div>
        <div id="form-control">
            <label for="number">Amount <br>
            (negative - expense,  positive - income)
            </label>
            <input type="text" id="number" v-model="amount" placeholder="please enter amount...">
        </div>
        <button class="btn">Add Transaction</button>
    </form>
</template>

<script setup>
import {ref} from 'vue'
import {useToast} from 'vue-toastification'

let text = ref('')
let amount = ref('')

const emit = defineEmits(['transactionSubmitted']);

const toast = useToast();

const onSubmit = () => {
    if(!text.value || !amount.value) {
        toast.error('Both field must be filled');
        return;
    }
    
    const transactionData = {
        text: text.value, 
        amount: parseFloat(amount.value)
    }

    emit('transactionSubmitted', transactionData);

    text.value = ''; 
    amount.value = '';
}
</script>