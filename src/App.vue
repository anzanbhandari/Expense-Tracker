<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <AddTransaction @transactionSubmitted="handleTransactioSubmitted" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue"
import Balance from "./components/Balance.vue"
import IncomeExpense from "./components/IncomeExpense.vue"
import TransactionList from "./components/TransactionList.vue"
import AddTransaction from "./components/AddTransaction.vue"

import {useToast} from 'vue-toastification';
import {ref, computed} from 'vue'

const toast = useToast();

//get transaction 
 const transactions = ref([
    {id: 1, remarks: 'milk', amount: -130},
    {id: 2, remarks: 'salary', amount: 20000},
    {id: 3, remarks: 'mango', amount: -500},
    {id: 4, remarks: 'refund', amount: 10000},
])


//get total balance
const total = computed(() =>{
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
})

//get income
const income = computed(() =>{
  return transactions.value.filter((transaction) => transaction.amount > 0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0)
  .toFixed(2);
})

//get expenses
const expenses = computed(() =>{
  return transactions.value.filter((transaction) => transaction.amount < 0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0)
  .toFixed(2);
})

//Add transaction
const handleTransactioSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      remarks: transactionData.text,
      amount: transactionData.amount
    })
    toast.success('Transaction added successfully!')
}

//Generate unique id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 10000 );
}

//Transaction Deleted
const handleTransactionDeleted = (id) => {
  transaction.value = transactions.value.filter((transaction) => transaction.id !== id);
  toast.success('Transaction Deleted')
}

</script>