<template>
  <div class="page-company">
    <VSkeletonLoader v-if="$fetchState.pending" type="card" />
    <VCard v-else>
      <VImg
        v-for="photo in company.photos"
        :key="photo"
        :src="company.photos[0]"
        height="250"
        class="white--text"
        gradient="to top, rgba(0,0,0,.1), rgba(0,0,0,.5)"
      >
        <VCardTitle class="text-h3"
          >{{ company.name }} - {{ averageRating }}</VCardTitle
        >
      </VImg>
      <VCardText>
        <div class="page-company__contact-info">
          <VaCompanyLocation :location="company.location" />
          <div>
            <PhPhone size="24" />
            <p class="text-body-1">
              {{ company.phone | formatPhoneNumber }}
            </p>
          </div>
        </div>
      </VCardText>
    </VCard>
    <aside>
      <VAlert text>
        TODO: PATCH – this should go under the company card on small device
        <br />
        Remove the <code>VAlert</code> when it's done
      </VAlert>
      <ul class="pa-0">
        <VSkeletonLoader
          v-if="$fetchState.pending"
          type="article"
          elevation="2"
        />

        <v-container v-else>
          <v-row no-gutters>
            <v-col
              v-for="review in reviews"
              v-bind:review="review"
              v-bind:key="review.id"
              sm="12"
              class="pa-2"
            >
              <VaCompanyReview :review="review" />
            </v-col>
          </v-row>
        </v-container>
      </ul>
    </aside>
  </div>
</template>

<script>
import { PhPhone } from 'phosphor-vue'

import VaCompanyLocation from '~/components/company-location.vue'
import VaCompanyReview from '~/components/company-review.vue'

export default {
  name: `page-company`,
  components: { VaCompanyLocation, PhPhone, VaCompanyReview },
  data() {
    return {
      company: {},
      currentSlide: 0,
      reviews: [],
    }
  },
  async fetch() {
    try {
      const { params } = this.$route
      const company = await this.$axios.$get(`/companies/${params.id}`)
      this.company = company
      this.reviews = company.reviews
    } catch (error) {
      this.$nuxt.error(error)
    }
  },
  computed: {
    averageRating() {
      if (
        !Object.keys(this.company).includes('reviews') ||
        !Array.isArray(this.reviews) ||
        this.reviews.length === 0
      ) {
        return
      }
      const average =
        this.reviews.reduce((a, b) => {
          return a + b.rating
        }, 0) / this.reviews.length

      return average.toFixed(2)
    },
  },
  filters: {
    formatPhoneNumber: (nb) => `
        ${nb.substr(0, 3)} ${nb.substr(3, 2)} ${nb.substr(5, 2)} ${nb.substr(
      7,
      2,
    )} ${nb.substr(9, 2)} ${nb.substr(11, 2)}`,
  },
}
</script>

<style lang="scss" scoped>
.page-company {
  display: grid;
  grid-template-columns: minmax(0, 1fr) 16rem;
  grid-gap: var(--gutter);
}
.page-company__contact-info {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  grid-gap: var(--gutter);
}
</style>
