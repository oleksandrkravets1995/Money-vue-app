<template>
  <div>
    <div class="page-title">
      <h3>{{ 'Bill' | localize }}</h3>

      <button
        class="btn waves-effect waves-light btn-small btn-black-bcgr"
        @click="refresh"
      >
        <i class="material-icons">refresh</i>
      </button>
    </div>

    <div class="center-own" v-if="loading">
      <Loader />
    </div>

    <div v-else class="row">
      <HomeBill :rates="currency.rates" />

      <HomeCurrency :rates="currency.rates" :date="currency.date" />
    </div>
  </div>
</template>

<script>
import HomeBill from '@/components/HomeBill';
import HomeCurrency from '@/components/HomeCurrency';
export default {
  name: 'home',
  metaInfo() {
    return {
      title: this.$title('Menu_Bill')
    };
  },
  data: () => ({
    loading: true,
    currency: null
  }),
  async mounted() {
    if (!Object.keys(this.$store.getters.currency).length) {
      this.currency = await this.$store.dispatch('fetchCurrency');
    } else {
      this.currency = this.$store.getters.currency;
    }

    this.loading = false;
  },
  methods: {
    async refresh() {
      this.loading = true;
      this.currency = await this.$store.dispatch('fetchCurrency');
      this.loading = false;
    }
  },
  components: {
    HomeBill,
    HomeCurrency
  }
};
</script>
