<script setup>
const { formatDate } = useHelpers();
const orders = ref(null);
onMounted(async () => {
  const { customer } = await GqlGetOrders();
  orders.value = customer?.orders;
});
</script>

<template>
  <div class="bg-white rounded-lg flex shadow min-h-[250px] p-8 justify-center items-center">
    <table v-if="orders" class="w-full text-left table-auto">
      <thead>
        <tr>
          <th>{{ $t('messages.shop.order') }}</th>
          <th>{{ $t('messages.general.date') }}</th>
          <th>{{ $t('messages.general.status') }}</th>
          <th class="text-right">{{ $t('messages.shop.total') }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="order in orders.nodes" :key="order.orderNumber">
          <td class="rounded-l-lg">
            <NuxtLink :to="`/order-summary/${order.orderNumber}`" class="cursor-pointer hover:underline">
              {{ order.orderNumber }}
            </NuxtLink>
          </td>
          <td>{{ formatDate(order.date) }}</td>
          <td :class="`order-${order.status}`" class="order-status">
            {{ order.status }}
          </td>
          <td class="text-right rounded-r-lg">
            {{ order.total }}
          </td>
        </tr>
      </tbody>
    </table>
    <LoadingIcon v-else size="24" stroke="2" />
  </div>
</template>

<style lang="postcss" scoped>
tbody tr:nth-child(odd) {
  background-color: #fafafa;
}

tbody tr {
  @apply text-sm text-gray-500 hover:text-gray-800;
}

td,
th {
  @apply py-2 px-3;
}

.order-status {
  @apply border rounded-md font-semibold bg-gray-100 my-2 mx-3 text-xs leading-none p-1.5 inline-block;
}

.order-COMPLETED {
  @apply bg-green-50 border-green-100 text-green-600;
}

.order-CANCELLED {
  @apply bg-red-50 border-red-100 text-red-600;
}

.order-PENDING {
  @apply bg-yellow-50 border-yellow-100 text-yellow-600;
}

.order-PROCESSING {
  @apply bg-blue-50 border-blue-100 text-blue-600;
}
</style>
