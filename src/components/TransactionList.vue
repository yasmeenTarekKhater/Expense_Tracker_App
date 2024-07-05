<script setup>
defineProps({
    transactions:{
        type:Array,
        required:true
    }
});
const emit=defineEmits(['deleteTransaction']);
// -- handle delete one transaction ----------------
const handleDeleteTransaction=(transactionId)=>{
    emit('deleteTransaction',transactionId);
};

</script>
<template>
    <h3>History</h3>
    <ul v-if="transactions.length > 0"  id="list" class="list">
      <li v-for="transaction in transactions" :key="transaction.id"
      :class="+transaction.amount < 0 ? 'minus' : 'plus'">
        {{ transaction.text }} <span>$ {{ transaction.amount }}</span
        ><button class="delete-btn" @click="handleDeleteTransaction(transaction.id)">
          x
        </button>
      </li>
    </ul>
    <div v-else class="no-history"> No History </div>
  </template>