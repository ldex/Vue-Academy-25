<template>
    <div>
        <section v-if="error">
            {{ error.message }}
        </section>
        <section v-else>
            <div v-if="loading">
                <h2 class="loading">Loading products...</h2>
            </div>
            <div v-else>
                <product-list :products="products" :page-size="5">
                    <template v-slot="slotProps">
                        <span>{{ slotProps.product.name }}</span> <span>({{ slotProps.product.price }}$)</span>
                    </template>
                </product-list>
            </div>

        </section>
    </div>
</template>

<script>
import ProductList from '@/components/ProductList.vue';
import { mapState, mapActions } from 'vuex';

export default {
    name: 'app',
    components: {
        ProductList
    },
    data() {
        return {
            error: null
        }
    },
    computed: {
        ...mapState(['products']), // map `this.products` to `this.$store.state.products`
        ...mapState({ loading: 'isLoading' }) // map `this.loading` to `this.$store.state.isLoading`
    },
    created() {
        this.fetchProducts();
    },
    methods: {
        ...mapActions(['fetchProducts']) // map `this.fetchProducts()` to `this.$store.dispatch('fetchProducts')`
    },
    errorCaptured: function (error) {
        console.error('Error in component: ', error.message);
    },
}
</script>

<style lang="scss" scoped>

</style>