<template>
  <div
    class="fixed z-10 inset-0 overflow-y-auto touch-manipulation"
    aria-labelledby="modal-title"
    role="dialog"
    aria-modal="true"
    v-show="show"
  >
    <div
      class="flex items-center justify-center min-h-screen pt-4 px-4 text-center sm:block sm:p-0"
    >
      <div
        class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"
        aria-hidden="true"
        @click="$emit('onCloseFormModal')"
      ></div>
      <span
        class="hidden sm:inline-block sm:align-middle sm:h-screen"
        aria-hidden="true"
        >&#8203;</span
      >
      <div
        class="bg-[#2C2C2C] max-h-[80%] w-[95%] mx-auto inline-block align-bottom rounded-lg px-1 pt-5 pb-4 text-left overflow-auto shadow-xl transform transition-all sm:my-8 sm:align-middle"
        :class="size === 'large' ? 'lg:w-1/2' : 'lg:w-1/3'"
      >
        <div class="flex w-full">
          <div
            class="w-full flex justify-start px-4 items-center"
            v-if="returnNeeded"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 448 512"
              class="h-6 w-6 cursor-pointer text-gray-500"
              fill="currentColor"
              aria-hidden="true"
              @click="$emit('return')"
            >
              <path
                d="M9.4 233.4c-12.5 12.5-12.5 32.8 0 45.3l160 160c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L109.2 288 416 288c17.7 0 32-14.3 32-32s-14.3-32-32-32l-306.7 0L214.6 118.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0l-160 160z"
              />
            </svg>
          </div>
          <div class="w-full flex justify-end px-4">
            <svg
              class="h-8 w-8 cursor-pointer text-gray-500"
              viewBox="0 0 20 20"
              fill="currentColor"
              aria-hidden="true"
              @click="$emit('onCloseFormModal')"
            >
              <path
                d="M6.28 5.22a.75.75 0 00-1.06 1.06L8.94 10l-3.72 3.72a.75.75 0 101.06 1.06L10 11.06l3.72 3.72a.75.75 0 101.06-1.06L11.06 10l3.72-3.72a.75.75 0 00-1.06-1.06L10 8.94 6.28 5.22z"
              ></path>
            </svg>
          </div>
        </div>
        <div class="text-center text-2xl font-semibold">Panier</div>

        <div class="flex flex-col">
          <div v-for="item in cart" :key="item.id">
            <ul class="">
              <li class="py-2">
                <div class="flex justify-between px-10">
                  <div class="flex flex-row items-center">
                    <img
                      :src="item.image"
                      :alt="item.subcategory"
                      class="w-24 h-24 py-1 mr-5"
                    />
                    <div class="text-center">
                      <p class="py-1">{{ item.name }}</p>
                      <p class="py-1">Quantité: {{ item.quantity }}</p>
                    </div>
                  </div>

                  <p class="py-1 flex items-center underline">
                    Prix: {{ item.price * item.quantity }}€
                  </p>
                </div>
              </li>
            </ul>
            <div
              v-if="item"
              class="flex cursor-pointer justify-center text-xl py-5"
            >
              <button
                @click="showWarnNotification()"
                class="p-1 px-6 bg-green-600 border border-green-600 text-white rounded-lg hover:bg-green-700"
              >
                Payer
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { notify } from "@kyvg/vue3-notification";

defineProps({
  show: {
    type: Boolean,
    default: false,
  },
  size: {
    type: String,
    default: "large",
  },
  returnNeeded: {
    type: Boolean,
    default: false,
  },
  cart: {
    type: Array,
    default: [],
  },
});

const showWarnNotification = () => {
  notify({
    title: "To do",
    type: "warn", // "success", "error", "warn"
    pauseOnHover: true,
  });
};
defineEmits(["onCloseFormModal", "return"]);
</script>
