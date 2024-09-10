<template>
  <div class="overflow-x-auto col-span-12">

    <table class="w-full  !overflow-x-scroll">
      <!-- head -->
      <thead v-if="showHead" class="bg-primary  h-[3.5rem] text-xs ">
      <tr>
        <th v-for="(item,idx) in props.fields" :key="idx"
            :class="{'!rounded-l-xl':idx===props.fields.length-1,'!rounded-r-xl':idx===0}"
            class="min-w-[5rem] text-white">{{ item['label'] }}
        </th>
      </tr>
      </thead>
      <tbody class="text-xs md:text-base">
      <template v-if="props.items.length>0">
        <tr v-for="(row,idx) in props.items" :key="idx" class="border">
          <td v-for="(td,idx) in props.fields" :key="idx">
            <slot :items="row" :name="td?.key">
              <span v-if="row[td?.key]">{{ td.formatter ? td.formatter(row[td.key]) : row[td?.key] }}</span>
              <span v-else>-</span>
            </slot>
          </td>
        </tr>
      </template>
      <span v-else>{{ props.emptyText }}</span>
      </tbody>
    </table>
  </div>

</template>

<script lang="ts" setup>
import type {PropType} from "vue";

const props = defineProps({
  items: {
    type: Object as PropType<object>,
    required: true
  },
  fields: {
    type: Object as PropType<Record<string, string>[]>,
    required: true
  },
  showHead: {
    type: Boolean as PropType<boolean>,
    default: true
  },
  emptyText: {
    type: String,
    required: false,
    default: 'موردی یافت نشد'
  }
})

function computedColors(idx: number) {
  if (idx % 2 === 0) {
    return '!bg-white  dark:!bg-blue-800'
  } else {
    return '!bg-blue-200 dark:!bg-blue-700'
  }

}


</script>
<style scoped>
table {
  vertical-align: center;
  text-align: center;
}

table td {
  line-height: 3rem !important;
}

table thead th {
  @apply dark:text-white;
}

table tbody td {
  line-height: 2.5rem !important;
  vertical-align: center !important;
  horiz-align: center !important;

  @apply dark:text-white;
}
</style>
