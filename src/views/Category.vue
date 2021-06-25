<template>
    <div class="page-category">
        <div class="columns is-multiline mt-3">
            <div class="column is-12">
                <h2 class="is-size-2 has-text-centered">{{ category.name }}</h2>
            </div>

            <div 
                class="column is-3"
                v-for="product in category.products"
                v-bind:key="product.id"
                > 
                <div class="box">
                    <figure class="image mb-4">
                    <img :src="product.get_thumbnail">
                    </figure>
                    
                    <h3 class="is-size-4">{{product.name}}</h3>
                    <p class="is-size-4 has-text-grey">${{product.price}}</p>

                    <router-link v-bind:to="product.get_absolute_url" class="button is-dark mt-4">Buy Now</router-link>
                </div>


            </div>

            <!-- <ProductBox 
                v-for="product in category.products"
                v-bind:key="product.id"
                v-bind:product="product" /> -->
        </div>
    </div>
</template>

<script>
import axios from "axios"
import { toast } from 'bulma-toast'

export default {
    name: 'Category',
    components: {
        
    },
    data() {
        return {
            category: {
                products: []
            }
        }
    },
    mounted() {
        this.getCategory()
    },
    watch: {
        $route(to, from) {
            if (to.name === 'Category') {
                this.getCategory()
            }
        }
    },
    methods: {
        async getCategory() {
            const categorySlug = this.$route.params.category_slug
            this.$store.commit('setIsLoading', true)
            axios
                .get(`/api/products/${categorySlug}/`)
                .then(response => {
                    this.category = response.data
                    console.log("For testing")
                })
                .catch(error => {
                    console.log(error)
                    toast({
                        message: 'Something went wrong. Please try again.',
                        type: 'is-danger',
                        dismissible: true,
                        pauseOnHover: true,
                        duration: 2000,
                        position: 'bottom-right',
                    })
                })
            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>