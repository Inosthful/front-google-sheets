<template>
  <div class="">
    <p class="flex justify-center text-center text-4xl underline font-bold">
      Google sheets
      <span class="pl-2 w-12 cursor-pointer" @click="$emit('openModal')">
        <img
          src="https://www.svgrepo.com/show/80543/shopping-cart-outline.svg"
          alt=""
      /></span>
    </p>
    <div class="gap-10 p-4 grid grid-cols-2 w-full">
      <div
        v-for="category in data"
        :key="category.category"
        class="border border-gray-400 shadow-sm rounded-xl gap-4 p-4 w-full"
      >
        <h2 class="text-center text-3xl font-semibold pb-3">
          {{ category.category }}
        </h2>
        <div class="grid grid-cols-1">
          <div
            v-for="subcategory in category.subcategory"
            :key="subcategory.subcategory"
            class=""
          >
            <h3 class="text-center justify-center text-xl underline pb-6">
              {{ subcategory.subcategory }}
            </h3>

            <ul class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3">
              <li
                class="flex flex-col justify-center items-center"
                v-for="item in subcategory.items"
                :key="item.id"
              >
                <img
                  :src="item.image"
                  :alt="item.subcategory"
                  class="w-48 h-48 py-1 rounded-xl"
                />
                <div class="py-1">{{ item.subcategory }}</div>
                <div class="">
                  <select
                    name=""
                    id=""
                    @change="
                      (e) => (selectedQuantity[item.id] = e.target.value)
                    "
                  >
                    <option
                      v-for="quantity in Array.from(
                        { length: item.quantity },
                        (_, i) => i + 1
                      )"
                      :key="quantity"
                      :value="quantity"
                    >
                      {{ quantity }}
                    </option>
                  </select>
                </div>
                <div class="py-1">Prix: {{ item.price }}</div>
                <div class="">
                  <button
                    :disabled="item.quantity === 0"
                    @click="
                      item.quantity > 0
                        ? ($emit('updateCart', {
                            ...item,
                            quantity: Number(selectedQuantity[item.id]) || 1,
                          }),
                          (item.quantity -= Number(selectedQuantity[item.id])),
                          showSuccessNotification())
                        : showWarnNotification()
                    "
                    class="cursor-pointer px-3 md:px-3 py-1 mb-3 md:py-1 bg-grey-800 border border-sky-600 text-white rounded-lg hover:bg-sky-700"
                    :class="
                      item.quantity === 0
                        ? 'cursor-not-allowed'
                        : 'cursor pointer'
                    "
                  >
                    Acheter
                  </button>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import axios from "axios";
import { notify } from "@kyvg/vue3-notification";

const showSuccessNotification = () => {
  notify({
    title: "Article ajouté au panier",
    type: "success", // "success", "error", "warn"
    pauseOnHover: true,
  });
};
const showErrorNotification = () => {
  notify({
    title: "Erreur",
    type: "error", // "success", "error", "warn"
    pauseOnHover: true,
  });
};
const selectedQuantity = ref({});
const data = ref(null);
onMounted(async () => {
  try {
    const response = await axios.get("http://localhost:3000/items");
    data.value = response.data.data;
    // selectedQuantity.value =
  } catch (error) {
    console.error("Erreur lors de l'appel API:", error);
  }
});

defineEmits(["openModal", "updateCart"]);
</script>

<style scoped></style>
