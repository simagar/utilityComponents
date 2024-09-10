<template>
  <div class="w-full flex flex-col xl:p-0 p-3 ">
    <div class="w-full flex xl:flex-row flex-col xl:items-center gap-4 justify-start ">
      <strong v-if="tabTitle" class="text-2xl  ">{{ tabTitle }}</strong>
      <div class="xl:w-3/4 w-full flex items-center  justify-start">
        <div v-for="(tab,idx) in props.tabs" :key="idx" :class="{'active-tab':activeTab===idx}"
             class="border-b-2 min-w-[4rem] flex items-center justify-center cursor-pointer bg-gray-200 rounded-t-xl  p-3 "
             @click="emitActiveTab(idx)">
          {{ tab.label }}
        </div>
      </div>
    </div>
    <template v-for="(tab,idx) in props.tabs">
      <div v-if="activeTab === idx" class="w-full tab-height relative ">
        <slot :name="tab.key"></slot>
      </div>
    </template>
  </div>
</template>

<script lang="ts" setup>
import type PropType from 'vue'
import {onMounted} from "vue";

const props = defineProps({
  tabs: {
    type: Array as PropType<Record<string, string>[]>
  },
  firstActiveTab: {
    type: Number as PropType<null | number>
  },
  tabTitle: {
    type: String as PropType<null | string>,
    required: false
  }
})
const emits = defineEmits<{
  (e: 'clicked', tab: object): void
}>()
onMounted(() => {
  if (props.firstActiveTab) {
    activeTab.value = props.firstActiveTab
  }
})
let activeTab = ref(0)

function emitActiveTab(idx: number) {
  activeTab.value = idx
  emits('clicked', props.tabs[idx])
}
</script>

<style scoped>
.tab-height {
  min-height: calc(100vh - 180px);
}

.active-tab {
  @apply border-secondary text-white bg-primary font-semibold
}
</style>
