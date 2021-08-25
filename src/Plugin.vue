<template>
  <div class="integration">
    <div v-if="!modalIsOpen">
      <SfCarousel
        v-if="model.product"
        :settings="{ peek: 16, breakpoints: { 1023: { peek: 0, perView: 4 } } }"
        class="carousel"
      >
        <!-- for some reason, id doesn't recognize SfCarouselItem component -->
        <!-- <SfCarouselItem
          v-for="(product, index) in model.products"
          :key="index"  
        > -->
        <IntegrationItem
          v-for="(product, index) in model.products"
          :key="index"
          :product="product"
          :current="current"
          @select="selectItem"
        />
        <!-- </SfCarouselItem> -->
      </SfCarousel>
      <button class="uk-button uk-width-1-1 uk-margin-small-top" @click.prevent="openSelection">Select products</button>
    </div>

    <div v-if="modalIsOpen">
      <IntegrationSelection
        :options="options"
        :current="model.product"
        @select="selectItem"
        @close="closeSelection"
      />      
    </div>

  </div>
</template>

<script>
import IntegrationItem from './IntegrationItem'
import IntegrationSelection from './IntegrationSelection'
import { SfCarousel } from '@storefront-ui/vue'

export default {
  mixins: [window.Storyblok.plugin],
  components: {
    SfCarousel,
    IntegrationItem,
    IntegrationSelection
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
        products: null,
      }
    },
    pluginCreated() {
      this.checkOptions()
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
    checkOptions() {
      if (typeof this.options.endpoint === 'undefined') {
        // eslint-disable-next-line
        console.error(`Please provide the option 'endpoint' with your endpoint url as value`)
      }
      if (typeof this.options.token === 'undefined') {
        // eslint-disable-next-line
        console.info(`If you need an authentication using Basic Auth add a option with the name token.`)
        // eslint-disable-next-line
        console.info(`Your provided token will be used as username performing a Basic Auth, the password will be left blank`)
      }
    }
  },
}
</script>
