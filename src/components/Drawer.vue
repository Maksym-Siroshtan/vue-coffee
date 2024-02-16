<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import DrawerFooter from './DrawerFooter.vue'

import InfoBlock from './InfoBlock.vue'

defineProps({
  cartIsEmpty: Boolean,
  orderId: Number
})
</script>

<template>
  <div class="fixed top-0 right-0 bottom-0 left-0 bg-black z-10 opacity-60"></div>
  <div class="fixed top-0 right-0 flex flex-col overflow-auto w-1/4 h-screen bg-white p-7 z-20">
    <div class="mb-7">
      <DrawerHead />
    </div>

    <div v-if="cartIsEmpty || orderId" class="flex flex-col justify-center h-full">
      <div v-if="cartIsEmpty && !orderId">
        <InfoBlock
          title="Кошик порожній"
          description="Додайте хоча б одну пачку кави, щоб зробити замовлення."
          image-url="/package-icon.png"
        />
      </div>
      <div v-if="!cartIsEmpty && orderId">
        <InfoBlock
          title="Замовлення оформлено!"
          :description="`Ваше замовлення #${orderId} скоро буде передано кур'єрській доставці.`"
          image-url="/order-success-icon.png"
        />
      </div>
    </div>

    <div v-else class="flex flex-col h-full">
      <div class="flex-1">
        <CartItemList />
      </div>

      <div class="mt-10">
        <DrawerFooter />
      </div>
    </div>
  </div>
</template>
