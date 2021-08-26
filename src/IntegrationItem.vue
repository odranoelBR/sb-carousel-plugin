<template>
  <div class="integration-item">
    <p>{{ product.name }}</p>
    <SfProductCard
      :image="product.image"
      :title="product.name"
      :link="`https://storefrontui.io/${product.slug}`"
      :regular-price="product.listPrice"
      special-price=""
      score-rating=""
      max-rating=""
      is-on-wishlist=""
      show-add-to-cart-button
      reviews-count=""
      badge-label=""
      badge-color=""
      wishlist-icon="heart"
      @click="selectItem(product)"
    />
  </div>
</template>

<script>
import "@storefront-ui/vue"
import { SfProductCard } from '@storefront-ui/vue'

export default {
  props: {
    current: Object,
    product: Object
  },
  components: {
    SfProductCard
  },
  mounted() {
    // eslint-disable-next-line
    console.log(this.product)
    // eslint-disable-next-line
    console.log(Object.keys(this.product))
  },
  methods: {
    selectItem(product) {
      this.$emit('select', product);
    },
  },
};
</script>

<style lang="scss">
@import "@storefront-ui/vue/styles.scss";

.integration-item {
  margin: 0 20px;
}
</style>
