<script setup>
import { ref ,computed} from 'vue';
import { useToast } from "vue-toastification";

import AddTransaction from './components/AddTransaction.vue';
import Balance from './components/Balance.vue';
import Header from './components/Header.vue'
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import { onMounted } from 'vue';

const toast = useToast();

const transactions=ref([]);
onMounted(()=>{
  let savedTransactions=JSON.parse(localStorage.getItem("transactions"));
  if(savedTransactions){
    transactions.value=savedTransactions;
  }
});
//  ---------------- get total income expense ------------
const calculations = computed(() => {
    return transactions.value.reduce(
        (acc, curr) => {
            acc.total += curr.amount;
            if (curr.amount > 0) {
                acc.income += curr.amount;
            } else {
                acc.expense += curr.amount;
            }
            return acc;
        },
        { total: 0, income: 0, expense: 0 }
    );
});
//---------------------- handle add new transaction ---------
const handleAddedTransaction=(transactionsData)=>{
  transactions.value.push({...transactionsData,id:generateId()});
  savedTransactionToLocalStorage();
  toast.success("addded successfully")

}
const generateId=()=>{
  return Math.floor(Math.random()*1000);
}
//---------------------- handle delete one transaction ---------
const handleDeletedTransaction=(deletedId)=>{
  transactions.value = transactions.value.filter(
    transaction=>transaction.id !== deletedId
  );
  savedTransactionToLocalStorage();
  toast.success("Transaction Deleted");
}
//---------------------- handle saved transaction to local storage ---------
const savedTransactionToLocalStorage=()=>{
  localStorage.setItem("transactions",JSON.stringify(transactions.value));
}
</script>

<template>
  <Header/>
  <div class="container">
    <Balance :total="calculations.total"/>
    <IncomeExpenses :income="calculations.income" :expense="calculations.expense"/>
    <TransactionList :transactions="transactions" @deleteTransaction="handleDeletedTransaction($event)"/>
    <AddTransaction @addNewTransaction="handleAddedTransaction($event)"/>
  </div>
</template>
