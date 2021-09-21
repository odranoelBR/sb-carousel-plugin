<template>
  <div class="integration-selection">
    <div
      class="uk-flex uk-flex-middle util__grow integration-selection__header"
    >
      <div class="util__grow">
        <form class="uk-margin-right" @submit.prevent="search">
          <input
            @input="debouncedSearch"
            v-model="search_term"
            placeholder="Search"
            class="uk-width-1-1"
          />
        </form>
      </div>
      <div class="uk-position-relative uk-text-nowrap">
        <div slot="actions">
          <a class="uk-button" @click="close">
            <i class="uk-icon-close"></i> Close
          </a>
        </div>
      </div>
    </div>
    <div class="spinner" v-if="loading" />
    <div v-if="!loading && products.length" class="integration-items">
      <div v-for="(product, index) in products" :key="index">
        <ProductItem :product="product" @select="selectItem" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import debounce from "debounce";
import ProductItem from "./ProductItem";

export default {
  components: {
    ProductItem
  },
  props: {
    options: Object
  },
  data() {
    return {
      search_term: "",
      page: 1,
      products: [],
      loading: true
    };
  },
  computed: {
    requestOptions() {
      let config = { params: { page: this.page } };
      if (this.search_term.length > 0) {
        config.params.search = this.search_term;
      }
      if (typeof this.options.token !== "undefined") {
        config.auth = {
          username: this.options.token
        };
      }
      return config;
    }
  },
  created() {
    this.load();
  },
  methods: {
    debouncedSearch: debounce(function() {
      this.search();
    }, 300),
    search() {
      this.page = 1;
      this.load();
    },
    load() {
      this.loading = true;
      let query = "";
      if (this.search_term) {
        query = `{\n allProductTemplates (name: "${this.search_term}") {\n id \n name \n image \n slug \n listPrice\n }\n}\n`;
      } else {
        query =
          "{\n allProductTemplates {\n id\n name\n displayName\n image\n slug\n listPrice\n description\n defaultCode\n standardPrice\n currency { name\n symbol\n }\n firstVariantId }\n}\n";
      }
      axios({
        url: "https://vsf.labs.odoogap.com/api/odoo/getProductTemplatesList",
        method: "post",
        headers: {
          "Content-Type": "application/json"
        },
        data: {
          operationName: null,
          query: query,
          variables: {}
        }
      }).then(res => {
        // eslint-disable-next-line no-console
        console.log(res);
        this.products = res.data.products.products;
        this.loading = false;
      });
    },
    close() {
      this.$emit("close");
    },
    selectItem(product) {
      this.$emit("select", product);
    }
  }
};
</script>

<style lang="scss">
.integration-selection {
  min-height: 700px;
  overflow-x: hidden;
  overflow-y: auto;
}

.spinner {
  width: 50px;
  height: 50px;
  background-color: #09b3af;

  margin: 100px auto;
  -webkit-animation: sk-rotateplane 1.2s infinite ease-in-out;
  animation: sk-rotateplane 1.2s infinite ease-in-out;
}

@-webkit-keyframes sk-rotateplane {
  0% {
    -webkit-transform: perspective(120px);
  }
  50% {
    -webkit-transform: perspective(120px) rotateY(180deg);
  }
  100% {
    -webkit-transform: perspective(120px) rotateY(180deg) rotateX(180deg);
  }
}

@keyframes sk-rotateplane {
  0% {
    transform: perspective(120px) rotateX(0deg) rotateY(0deg);
    -webkit-transform: perspective(120px) rotateX(0deg) rotateY(0deg);
  }
  50% {
    transform: perspective(120px) rotateX(-180.1deg) rotateY(0deg);
    -webkit-transform: perspective(120px) rotateX(-180.1deg) rotateY(0deg);
  }
  100% {
    transform: perspective(120px) rotateX(-180deg) rotateY(-179.9deg);
    -webkit-transform: perspective(120px) rotateX(-180deg) rotateY(-179.9deg);
  }
}

.integration-selection__header {
  position: sticky;
  top: 0px;
  background: #ffffff;
  padding-bottom: 10px;
}

.integration-items {
  display: grid;
  grid-template-columns: repeat(auto-fill, 154px);
  justify-content: center;
}
</style>
