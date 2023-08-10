<script setup>
  import { ref, watch } from "vue";
  import {
    Header,
    Buttons,
    SetAmount,
    PaymentDeadline,
    PaymentSummary
  } from './components';
  import { setAmountCurrency, setPayments }  from "./helpers";

  const minLoan            = 100;
  const maxLoan            = 30000;
  const step               = 100;
  const initialLoan        = 10000;

  let loanAmount           = ref(initialLoan);
  let deadline             = ref("");
  let loading              = ref(false);
  let totalPayment         = 0;
  let monthlyPayment       = 0;
  const deadlineOptions = [
        {
            value: "6",
            optionText: "6 meses"
        },
        {
            value: "12",
            optionText: "12 meses"
        },
        {
            value: "24",
            optionText: "24 meses"
        },
  ];

  const changeRange = (e) => {
    loanAmount.value = Number(e.target.value);
  };

  const clickDecrement = (isDecrement) => {
    if (isDecrement && loanAmount.value > minLoan) {
      loanAmount.value = loanAmount.value - step;
      return;
    }

    if (!isDecrement && loanAmount.value  < maxLoan) {
      loanAmount.value = loanAmount.value + step;
    }
  };

  watch([deadline, loanAmount], () => {
    loading.value = true;

    const payment = setPayments(loanAmount.value, deadline.value);

    if (payment) {
      totalPayment   = payment.fullPayment;
      monthlyPayment = payment.monthlyPayment;
      loading.value  = false;
    } else {
      alert("there was an error while calculating")
    }
  });
</script>

<template>
  <div class="bg-white shadow p-[10px] rounded-[10px] w-full">
    <Header />

    <SetAmount
      :amount="loanAmount"
      :min="minLoan"
      :max="maxLoan"
      :step="step"
      :setAmountCurrency="setAmountCurrency"
      :changeFunction="changeRange"
    />

    <div class="flex justify-between py-5">
      <Buttons
        :btnText="'-'"
        :isDecrement="true"
        @clickFunction="clickDecrement"
      />
      <Buttons
        :btnText="'+'"
        :isDecrement="false"
        @clickFunction="clickDecrement"
      />
    </div>

    <!-- <PaymentDeadline :deadlineSelected="deadline" @selectFunction="selectMonth" /> -->

    <div class="pb-5">
      <select
        class="p-2  w-1/2 shadow bg-white border border-gray-300 rounded-lg text-gray-500 sm:text-xl text-lg font-bold text-center"
        :value="deadline"
        v-model.number="deadline"
      >
        <option v-for="option in deadlineOptions" :value="option.value">{{ option.optionText }}</option>
      </select>
    </div>

    <!-- <PaymentSummary /> -->

    <div class="p-4 sm:w-3/5 w-full sm:m-auto bg-lime-100 rounded-lg">
      <h1 class="text-gray-500 font-bold text-2xl pb-1">Payment <span className="text-purple-600">summary</span></h1>

      <h1 class="text-purple-600 font-bold text-lg pb-1" v-if="loading">Loading...</h1>

      <p
        class="text-gray-500 font-bold text-lg md:text-left flex flex-col text-center md:block"
        v-if="totalPayment && !loading"
      >
        Selected period: &nbsp;
        <span class="text-purple-600">{{ deadline }} meses</span>
      </p>

      <p
        class="text-gray-500 font-bold text-lg md:text-left flex flex-col text-center md:block"
        v-if="totalPayment && !loading"
      >
        Total payment: &nbsp;
        <span class="text-purple-600">{{ totalPayment }}</span>
      </p>

      <p
        class="text-gray-500 font-bold text-lg md:text-left flex flex-col text-center md:block"
        v-if="totalPayment && !loading"
      >
        monthly payment: &nbsp;
        <span class="text-purple-600">{{ monthlyPayment }}</span>
      </p>
    
      <h1 class="text-purple-600 font-bold text-lg pb-1" v-if="!totalPayment && !loading">Select a period to start</h1>
    </div>
  </div>
</template>
