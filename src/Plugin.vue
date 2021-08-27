<template>
  <div class="integration">
    <div v-if="!modalIsOpen">
      <SfCarousel
        v-if="model.products"
        :settings="{ peek: 16, breakpoints: { 1023: { peek: 0, perView: 2 } } }"
        class="carousel"
      >
        <template #prev="{go}">
          <SfArrow
            aria-label="prev"
            class="sf-arrow--left sf-arrow--long"
            @click="go('prev')"
          />
        </template>
        <template #next="{go}">
          <SfArrow
            aria-label="next"
            class="sf-arrow--right sf-arrow--long"
            @click="go('next')"
          />
        </template>
        <!-- for some reason, id doesn't recognize SfCarouselItem component -->
        <!-- <SfCarouselItem
          v-for="(product, index) in model.products"
          :key="index"  
        > -->
        <ProductItem
          v-for="(product, index) in model.products"
          :key="index"
          :product="product"
          is-on-carousel
          @select="selectItem"
        />
        <!-- </SfCarouselItem> -->
      </SfCarousel>
      <button class="uk-button uk-width-1-1 uk-margin-small-top" @click.prevent="openSelection">Manage products</button>
    </div>

    <div v-if="modalIsOpen">
      <ProductSelection
        :options="options"
        @select="selectItem"
        @close="closeSelection"
      />      
    </div>
  </div>
</template>

<script>
import ProductItem from './ProductItem'
import ProductSelection from './ProductSelection'

import {
  SfCarousel,
  SfArrow,
} from '@storefront-ui/vue';

export default {
  mixins: [window.Storyblok.plugin],
  components: {
    SfCarousel,
    SfArrow,
    ProductItem,
    ProductSelection,
  },
  data() {
    return {
      modalIsOpen: false,
    }
  },
  watch: {
    'model': {
      handler(value) {
        this.$emit('changed-model', value);
      },
      deep: true
    }
  },
  methods: {
    initWith() {
      return {
        // needs to be equal to your storyblok plugin name
        plugin: 'odoo-carousel',
        products: [],
      }
    },
    openSelection() {
      this.modalIsOpen = true
      this.$emit('toggle-modal', true)
    },
    closeSelection() {
      this.modalIsOpen = false
      this.$emit('toggle-modal', false)
    },
    selectItem(product) {
      this.model.products.push(product)
    },
  },
}
</script>
