<script setup>
import { ref, computed, onMounted } from "vue";

import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { useToast } from "vue-toastification";

const toast = useToast();

//把local-storage的transactions賦值到savedTransaction
onMounted(() => {
  const savedTransaction = JSON.parse(localStorage.getItem("transactions"));

  if (savedTransaction) {
    transactions.value = savedTransaction;
  }
});

const transactions = ref([
  // {
  //   id: 1,
  //   text: "Flower",
  //   amount: -19.99,
  // },
  // {
  //   id: 2,
  //   text: "Salary",
  //   amount: 299.99,
  // },
  // {
  //   id: 3,
  //   text: "Book",
  //   amount: -10,
  // },
  // {
  //   id: 4,
  //   text: "Camera",
  //   amount: -150,
  // },
]);

//Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transactions) => {
    return acc + transactions.amount;
  }, 0);
});

//Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transactions) => {
      return acc + transactions.amount;
    }, 0);
});

//Get expense
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transactions) => {
      return acc + transactions.amount;
    }, 0);
});

//Add transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueID(),
    text: transactionData.text,
    amount: transactionData.amount,
  });

  saveTransactionToLocalStorage();

  toast.success("新增成功!");
};

//Get random ID
const generateUniqueID = () => {
  return Math.floor(Math.random() * 100000);
};

//Delete transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => {
    return transaction.id !== id;
  });

  saveTransactionToLocalStorage();

  toast.success("刪除成功!");
};

//Save to local-storage
const saveTransactionToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="income" :expenses="expenses" />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeleted"
    />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<style scoped></style>
