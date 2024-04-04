<template>
  <HelloWorld
    @openModal="ModalIsActive = true"
    @updateCart="updateCart($event)"
  />

  <Modal
    :show="ModalIsActive"
    @onCloseFormModal="ModalIsActive = false"
    :cart="cart"
  />
  <Notifications width="300" />
</template>

<script setup>
import HelloWorld from "./components/Base/HelloWorld.vue";
import { onMounted, ref } from "vue";
import Modal from "./components/Modals/Modal.vue";
import { Notifications } from "@kyvg/vue3-notification";

const ModalIsActive = ref(false);
const cart = ref([]);

const updateCart = (event) => {
  cart.value.find((item) => item.id === event.id)
    ? (cart.value.find((item) => item.id === event.id).quantity += Number(
        event.quantity
      ))
    : cart.value.push(event);
};
</script>
<style scoped></style>
