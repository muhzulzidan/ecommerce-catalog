<template>
    <div v-if="product" :class="['product-display', productClass]">
        <div v-if="loading" class="loader"></div>
         <div v-else class="product-content" :class="productClass">
            <div class="product-image-container">
                <img :src="product.image" :alt="product.title" class="product-image" />
            </div>
            <div class="product-details">
                <h2 class="product-title" :class="productClass">{{ product.title }}</h2>
                <p class="product-category">{{ product.category }}</p>
                <!-- Add the rating component here -->
                <p class="product-description">{{ product.description }}</p>
                <div class="product-purchase-info">
                    <p class="product-price">${{ product.price }}</p>
                    <div class="product-actions">
                        <button class="btn-buy" :class="productClass">Buy now</button>
                        <button @click="fetchNextProduct" class="btn-next" :class="productClass">Next product</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div v-else class="product-not-available">
        <div class="product-image-placeholder"></div>
        <div class="product-details-placeholder">
          <div class="product-title-placeholder">This product is unavailable to show</div>
          <div class="product-actions">
            <button @click="fetchNextProduct" class="btn-next unavailable">Next product</button>
          </div>
        </div>
      </div>
</template>


<script>
export default {
    data() {
        return {
            product: null,
            index: 1,
            loading: false,
        };
    },
    computed: {
        productClass() {
            if (!this.product) return 'unavailable';
            return {
                'men': this.product.category === "men's clothing",
                'women': this.product.category === "women's clothing"
            };
        },
    },

    mounted() {
        this.fetchProduct();
    },
    methods: {
        fetchProduct() {
            this.loading = true;
            console.log(this.index, "index")
            fetch(`https://fakestoreapi.com/products/${this.index}`)
                .then(response => response.json())
                .then(data => {
                    // Check if the product's category is men's or women's clothing
                    if (data.category === "men's clothing" || data.category === "women's clothing") {
                        this.product = data;
                    } else {
                        this.product = null; // Or handle unavailable products as you wish
                    }
                    this.loading = false;
                })
                .catch(error => {
                    console.error('Error fetching product:', error);
                    this.loading = false;
                });
        },
        fetchNextProduct() {
            this.index = this.index < 20 ? this.index + 1 : 1;
            this.fetchProduct();
        },
    },
};
</script>

