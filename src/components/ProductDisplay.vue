<template>
    <div v-if="product" :class="['product-display', productClass]">
        <div v-if="loading" class="loader"></div>
        <div v-else class="product-content">
            <img :src="product.image" :alt="product.title" class="product-image" />
            <div class="product-details">
                <h2 class="product-title">{{ product.title }}</h2>
                <p class="product-category">{{ product.category }}</p>
                <!-- Add the rating component here -->
                <p class="product-description">{{ product.description }}</p>
                <div class="product-purchase-info">
                    <p class="product-price">${{ product.price }}</p>
                    <div class="product-actions">
                        <button class="btn-buy">Buy now</button>
                        <button @click="fetchNextProduct" class="btn-next">Next product</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div v-else class="product-not-available">
        <p>Product not available.</p>
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
            return this.product.category === "men's clothing" ? 'men' : 'women';
        },
    },
    mounted() {
        this.fetchProduct();
    },
    methods: {
        fetchProduct() {
            this.loading = true;
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

