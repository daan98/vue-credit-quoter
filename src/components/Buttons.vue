<script setup>
    defineProps({
        btnText: String,
        isDecrement: Boolean
    });
</script>

<template>
    <button
      class=" bg-lime-300 rounded-full w-10 h-10 font-bold duration-300 transition-all ease-in-out hover:bg-lime-700 
      hover:text-white hover:duration-300 hover:transition-all hover:ease-out"
      @click="$emit('clickFunction', isDecrement)"
    >
      {{btnText}}
    </button>
</template>