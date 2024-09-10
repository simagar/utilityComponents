<template>
  <div>
    <input ref="mediaHandler" accept="image/*" class="hidden" type="file" @input="handleFile">
    <div v-if="mediaResult.pictureUrl ===''"
         class="w-24 h-24 bg-Gray rounded-lg shadow flex gap-3 items-center justify-end flex-col"
         @click="mediaHandler.click()">

      <small class="mb-2 text-secondary">
        آپلود عکس
      </small>
    </div>
    <div v-else class="w-24 h-24  rounded-lg shadow flex gap-3 items-center justify-end flex-col"
         @click="mediaHandler.click()">
      <img :src="mediaResult.pictureUrl" class="rounded-lg w-24 h-24  object-cover"/>
    </div>
  </div>
</template>

<script lang="ts" setup>

import {useRuntimeConfig} from "nuxt/app";

const config = useRuntimeConfig()
let mediaHandler = ref(null)
let mediaResult = ref({
  pictureUrl: '',
  base64: '',
  file: ''
})
const props = defineProps({
  defaultMedia: {
    type: String
  }
})
watch(() => props.defaultMedia, async (val) => {
  if (val) {
    mediaResult.value.pictureUrl = `${config.public.pictureUrl}/${val}`
  }
}, {immediate: true})
const emits = defineEmits<{
  (e: 'setMedia', MediaResult: object): void
}>()

function makeBase64(data: object | any): Promise<string> {
  return new Promise((resolve, reject) => {
    const fr = new FileReader();
    fr.onerror = reject;
    fr.onload = () => {
      // @ts-ignore
      resolve(fr?.result?.split(",")[1]);
    }
    fr.readAsDataURL(data);
  });
}

async function handleFile() {
  if (mediaHandler?.value?.files && mediaHandler?.value?.files[0]) {
    mediaResult.value.file = mediaHandler.value.files[0]
    mediaResult.value.pictureUrl = URL.createObjectURL(mediaHandler.value.files[0])
    mediaResult.value.base64 = await makeBase64(mediaHandler.value.files[0])
    emits('setMedia', mediaResult.value)
  }
}
</script>

<style scoped>

</style>
