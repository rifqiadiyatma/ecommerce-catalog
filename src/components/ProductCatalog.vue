<template>
  <div v-if="loading" class="lds-dual-ring"></div>
  <div v-else
    :class="product.category === `men's clothing` ? 'wrapper bg__man text__man' : product.category === `women's clothing` ? 'wrapper bg__woman text__woman' : 'wrapper bg__error'">
    <div class="card">
      <div v-if="isCategory === false" class="product-not__category">
        <img src="../assets/sad-face.svg" class="product-not__category__img">
        <div class="product-not__category__dtl">
          <p class="text__error">This product is unavailable to show</p>
          <div class="btn btn__error">
            <button type="button" @click="getDetailProduct()" class="btn__next">Next
              Product</button>
          </div>
        </div>
      </div>
      <div v-else class="product">
        <img :src="product.image" class="product__img">
        <div class="product__summary">
          <div class="product__title">{{ product.title }}</div>
          <div class="product__dtl">
            <div class="product__category">
              {{ product.category }}
            </div>
            <div class="product__rating">
              {{ rating }}/5
              <span class="dot"></span>
              <span class="dot"></span>
              <span class="dot"></span>
              <span class="dot"></span>
              <span class="dot"></span>
            </div>
          </div>
          <div class="product__description">
            {{ product.description }}
          </div>
          <div class="product__price">
            ${{ product.price }}
          </div>
          <div class="btn">
            <button type="button"
              :class="product.category === `men's clothing` ? 'btn__buy btn__man' : 'btn__buy btn__woman'">Buy
              Now</button>
            <button type="button" @click="getDetailProduct()"
              :class="product.category === `men's clothing` ? 'btn__next btn-outline__man' : 'btn__next btn-outline__woman'">Next
              Product</button>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: 'ProductCatalog',
  data() {
    return {
      product: {},
      id: 1,
      loading: true,
      isCategory: false,
      rating: 0,
    }
  },
  methods: {
    async getDetailProduct() {
      this.loading = true;
      if (this.id <= 20) {
        await fetch('https://fakestoreapi.com/products/' + this.id).
          then(response => response.json()).
          then(data => this.product = data);
        this.rating = this.product.rating.rate;
        this.product.category === `men's clothing` || this.product.category === `women's clothing` ? this.isCategory = true : this.isCategory = false;
        this.id++;
      } else {
        this.id = 1;
        await fetch('https://fakestoreapi.com/products/' + this.id).
          then(response => response.json()).
          then(data => this.product = data);
      }
      this.product.category === `men's clothing` || this.product.category === `women's clothing` ? this.isCategory = true : this.isCategory = false;
      this.loading = false;
    }
  },
  mounted() {
    this.getDetailProduct();
  }
}
</script>

<style scoped>
@import '../assets/styles/style.css';
</style>