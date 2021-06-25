<template>
  <div class="home">

      <div class="columns is-multiline mt-6">
        <div class="column is-12">
            <h2 class="is-size-2 has-text-centered">All products</h2>
        </div>
        
        <div 
          class="column is-3"
          v-for="product in latestProducts"
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
          v-for="product in latestProducts"
          v-bind:key="product.id"
          v-bind:product="product" /> -->
      </div>
  </div>
</template>

<script>
    import axios from 'axios'

    export default {
      name: 'Home',
      data() {
        return {
          latestProducts : []
        }
      },
      mounted() {
        this.getLatestProducts()
      },
      methods: {
        async getLatestProducts() {
          this.$store.commit('setIsLoading', true)

          await axios
            .get('/api/latest-products/')
            .then(response => {
              this.latestProducts = response.data
            })
            .catch(error => {
              console.log(error)
            }) 

          this.$store.commit('setIsLoading', false)
        }
      }
    }

</script>

<style scoped>
  .image {
    margin-top: -20px;
    margin-left: -20px;
    margin-right: -20px;
  }
</style>
