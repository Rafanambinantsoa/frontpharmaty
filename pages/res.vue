<script setup>

definePageMeta({
    layout: 'reservation',
})

useHead({
  title: 'EventPass - Reservation',
  meta: [
    { name: 'description', content: ' Ici vous ' },
  ],
  link: [
    { rel: 'icon', type: 'image/png', href: '/logo.png' }
  ],
})
</script>
<template>
    <ReservationForm />
</template>