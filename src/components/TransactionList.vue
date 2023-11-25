<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactions" 
            :key="transaction.id" 
            :class="transaction.amount < 0 ? 'minus' : 'plus'"> <!-- make for loop and must added key -->
            {{ transaction.text }} <span>Rp {{ transaction.amount }}</span><button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
        </li>
    </ul>
</template>

<script setup>
    import { defineProps } from 'vue'; //this include part of def
    const emit = defineEmits(['transactionDeleted']);
    const props = defineProps({
        transactions: {
            type: Array,
            required: true,
        },
    });

    const deleteTransaction = (id) => {
        emit('transactionDeleted', id);
    }
</script>