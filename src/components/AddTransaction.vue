<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
        <label for="text">Text</label>
        <input type="text" v-model="text" id="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
        <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
        >
        <!-- <input type="number" id="amount" placeholder="Enter amount..." /> -->
        <input type="text" v-model="amount" id="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
    </form>
</template>

<script setup>
    import { ref } from 'vue';
    import { useToast } from 'vue-toastification'; // view an alert

    const text = ref('');
    const amount = ref('');

    const emit = defineEmits(['transactionSubmitted']); // for update (you can rename it)

    const toast = useToast();

    const onSubmit = () => {
        if(!text.value || !amount.value) {
            toast.error('Both fileds must be filled');
            return;
        }

        const transactionData = {
            text: text.value,
            amount: parseFloat(amount.value)
        }

        emit('transactionSubmitted', transactionData); //emit name, data

        text.value = "";
        amount.value = "";
    }
</script>