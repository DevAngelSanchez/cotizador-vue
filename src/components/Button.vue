<script setup>
  const props = defineProps({
    operator: String,
  });
</script>

<template>
  <button
  type="button"
    class="h-10 w-10 flex items-center justify-center font-bold bg-sky-500 rounded-full hover:outline-none hover:ring-2 hover:ring-offset-2 hover:ring-sky-500 text-white text-2xl"
    @click="$emit('fn')"
  >
    {{ props.operator }}
  </button>
</template>