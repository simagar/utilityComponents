<template>
  <div class="flex w-full col-span-2 flex-col items-center justify-center gap-2">
    <img alt="" src="/no-result.webp">
    <span>{{ emptyText }}</span>
  </div>
</template>

<script lang="ts" setup>
import type PropType from "vue";

const props = defineProps({
  emptyText: {
    type: String as PropType<string>,
    required: false,
    default: 'جستجوی شما نتیجه ای نداشت'
  }
})
</script>

<style scoped>

</style>
