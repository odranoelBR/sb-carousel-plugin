<template>
  <div class="sf-product-card" @click="selectItem(product)">
    <div class="sf-product-card__image-wrapper">
      <SfImage
        class="sf-product-card__image"
        :src="product.image"
        :alt="product.description ? product.description : product.name"
      />
    </div>

    <div v-if="isOnCarousel">
      <SfButton
        :link="`https://vsfdemo.labs.odoogap.com/shop/${product.slug}`"
        class="sf-button--pure sf-product-card__link"
      >
        <h3 class="sf-product-card__title">{{ product.name }}</h3>
      </SfButton>
    </div>

    <h3 v-else class="sf-product-card__title">{{ product.name }}</h3>

    <SfPrice
      class="sf-product-card__price"
      :regular="product.listPrice"
      :special="product.listPrice"
    />

    <SfRating class="sf-product-card__rating" :max="5" :score="4" />

    <SfProductCard
      :image="product.image"
      :title="product.name"
      :link="`https://vsfdemo.labs.odoogap.com/shop/${product.slug}`"
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
import {
  // SfProductCard,
  SfImage,
  SfPrice,
  SfRating,
  SfButton
} from "@storefront-ui/vue";

export default {
  props: {
    product: Object,
    isOnCarousel: {
      type: Boolean,
      default: false
    }
  },
  components: {
    // SfProductCard,
    SfImage,
    SfPrice,
    SfRating,
    SfButton
  },
  methods: {
    selectItem(product) {
      this.$emit("select", product);
    }
  }
};
</script>

<style lang="scss">
@import "@storefront-ui/vue/styles.scss";

.integration-item {
  margin: 0 20px;
}
</style>
