<script setup lang="ts">
const props = defineProps<{
  context: string
  id: number
}>()
</script>

<template>
  <div h-80 w-50 flex select-none items-center justify-center transition-all border="~ rounded #333" :data-id="id" v-html="props.context" />
</template>
