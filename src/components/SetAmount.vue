<script setup>
    defineProps({
        min: Number,
        max: Number,
        step: Number,
        amount: Number,
        setAmountCurrency: Function,
        changeFunction: Function,
    });
</script>

<template>
    <div className='pt-5'>
      <p className="text-center text-purple-600 text-4xl font-extrabold pb-[10px]">{{ setAmountCurrency(amount) }}</p>
      
      <input
        type="range"
        className="w-full h-6 bg-gray-200 accent-lime-500 hover:accent-lime-600"
        :value="amount"
        :min="min"
        :max="max"
        :step="step"
        @input="e => changeFunction(e)"
      >
    </div>
</template>