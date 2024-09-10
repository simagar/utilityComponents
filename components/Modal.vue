<template>
  <!--  Modal region  -->
  <teleport to="body">
    <input :id="props.id" ref="close" class="modal-toggle" type="checkbox"/>

    <div :class="{'modal-bottom':props.modalBottom}" class="modal  z-[9999] " role="dialog">
      <label
          :class="[customWidth]"
          class="modal-box   relative  p-0 bg-white dark:bg-dark-muted"
          for=""
          style="direction: ltr !important"
      >
        <div
            class="flex items-center  justify-between px-4 pb-2 my-3 border-b dark:border-gray-400 border-gray-200"
        >
          <label :for="props.id">
            <i class="ri-close-circle-line text-red-500 text-xl cursor-pointer"></i>
          </label>
          <strong v-if="props.title" class="dark:text-white text-gray-700 text-sm md:text-lg">{{
              props.title
            }}</strong>
        </div>
        <slot v-if="hasBody" name="modalBody"></slot>
        <div
            v-if="!hideActions"
            class="!z-50 mt-3 space-x-3 flex px-3 pb-3"
        >
          <label
              v-if="props.okModalTitle"
              class="btn main-button  mb-2 border-none text-white"
              for="optionsModal"
              @click="ok"
          >
            <span class="mt-1">
              {{ props.okModalTitle }}
            </span>
          </label>
          <label
              class="btn secondary-button !text-white  mb-2 border-none"

              for="close"
              @click="closeModal"
          >
            <span class="">
              {{ props.closeModalTitle }}
            </span>
          </label>
        </div>
      </label>
      <label :for="props.id" class="modal-backdrop"></label>
    </div>
  </teleport>
  <!--  Modal region End -->
</template>

<script lang="ts" setup>
import {ref} from "vue";

const props = defineProps({
  id: {
    type: String,
  },
  customWidth: {
    type: String,
    default: 'w-full',

  },
  title: {
    type: String,
    default: null,
  },
  closeModalTitle: {
    type: String,
    required: false,
    default: "close",
  },
  okModalTitle: {
    type: String,
  },
  hasBody: {
    type: Boolean,
    default: true,
  },
  hideActions: {
    type: Boolean,
    default: false
  },
  modalBottom: {
    type: Boolean,
    default: false
  }
});


const emits = defineEmits<{
  (e: 'closeModal'): void,
  (e: 'ok'): void
}>();

const close = ref<any>(null);

function ok(): void {
  emits("ok");
  closeModal();
}

function closeModal(): void {
  close.value.click();
}
</script>

<style scoped></style>
