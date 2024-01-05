<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();
const text = ref("");
const amount = ref("");

const emit = defineEmits(["transactionSubmitted"]);

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error("項目與金額欄位皆須輸入");
    return;
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  };

  emit("transactionSubmitted", transactionData);

  text.value = "";
  amount.value = "";
};
</script>

<template>
  <h3>新增項目</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">項目</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >金額 <br />
        (輸入負值 - 支出，輸入正值 - 收入)</label
      >
      <input
        type="number"
        id="amount"
        v-model="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">新增</button>
  </form>
</template>

<style></style>
