<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" /> <!--to describe this is a number we add + sign-->
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" /> <!--passing data-->
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" /> <!--because receive data submit from this componen, so we write like this-->
  </div>
</template>

<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import {useToast} from 'vue-toastification';
  import { ref, computed, onMounted } from 'vue'; //this is for make data can change dinamis

  const toast = useToast();

  const transactions = ref([]);

  // statefull
  onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

    if (savedTransactions) {
      transactions.value = savedTransactions;
    }
  });

  /**
   * Get Total
   * this can run reactively
   */
  const total = computed(() => {
    // .value to make can call the data instanly with call its key
    return transactions.value.reduce((acc, transaction) => { 
      return acc + transaction.amount;
    }, 0); // set 0 to the initial value of accumulation. because if you don't define it, accumulation uses the first array as the initial value
  });

  /**
   * Get Income
   */
   const income = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount > 0) // arrow function
    .reduce((acc, transaction) => { 
      return acc + transaction.amount;
    }, 0)
    .toFixed(2); // has 2 digits behind the comma
  });

  /**
   * Get Expense
   */
   const expenses = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount < 0) // arrow function
    .reduce((acc, transaction) => { 
      return acc + transaction.amount;
    }, 0)
    .toFixed(2); // has 2 digits behind the comma
  });

  /**
   * Add transaction
   */
  const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text, // object value that send from component AddTransaction
      amount: transactionData.amount
    });

    saveTransactionToLocalStorage();

    toast.success('Transaction added');
  };

  /**
   * Generate ID
   */
  const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000);
  }

  /**
   * Delete Transaction
   */

  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id);

    saveTransactionToLocalStorage();

    toast.success('Transaction have deleted');
  }

  /**
   * Save to local storage
   */
  const saveTransactionToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
  }
</script>