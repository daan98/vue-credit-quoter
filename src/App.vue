<script setup>
import {
  Header,
  Buttons,
  SetAmount,
  PaymentDeadline,
  PaymentSummary
} from './components';
</script>

<template>
  <div class="">
    <Header />

    <SetAmount />

    <div class="flex justify-between py-5">
      <Buttons />
      <Buttons />
    </div>

    <PaymentDeadline />

    <PaymentSummary />
  </div>
</template>
