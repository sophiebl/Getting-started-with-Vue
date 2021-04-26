<template>
  <VCard class="va-company-card d-flex flex-column">
    <VImg
      height="100"
      :src="company.photos[0]"
      class="flex-shrink-0 flex-grow-0"
    />
    <VCardTitle class="headline">
      {{ company.name }}
    </VCardTitle>
    <div class="average">Average rating: {{ averageRating }}</div>

    <VCardText>
      <VaComponayLocation :location="company.location" />
    </VCardText>
    <VCardActions class="mt-auto">
      <VBtn
        color="primary"
        nuxt
        :to="`/companies/${company.id}`"
        block
        outlined
      >
        see more
      </VBtn>
    </VCardActions>
  </VCard>
</template>

<script>
import VaComponayLocation from '~/components/company-location.vue'

export default {
  name: `va-company-card`,
  components: { VaComponayLocation },
  props: {
    company: { type: Object, default: () => ({}) },
  },
  computed: {
    averageRating() {
      if (
        !Object.keys(this.company).includes('reviews') ||
        !Array.isArray(this.company.reviews) ||
        this.company.reviews.length === 0
      ) {
        return
      }

      const reviews = this.company.reviews
      const average =
        reviews.reduce((a, b) => {
          return a + b.rating
        }, 0) / reviews.length

      return average.toFixed(2)
    },
  },
}
</script>

<style lang="scss" scoped>
.average {
  width: 100%;
  text-align: center;
}
</style>
