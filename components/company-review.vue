<template>
  <VCard class="va-company-review" tag="li">
    <VCardText>{{ review.text }}</VCardText>
    <v-rating
      v-model="review.rating"
      background-color="grey lighten-2"
      color="yellow darken-3"
      half-increments
      hover
      readonly
    ></v-rating>
  </VCard>
</template>

<script>
export default {
  name: `va-company-review`,
  props: {
    review: { type: Object, default: () => ({}) },
  },
}
</script>
