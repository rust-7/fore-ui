<template>
    <div class="section">
        <div class="container is-fluid">
            <div class="columns">
                <div class="column is-half">
                    <img src="http://via.placeholder.com/620x620" alt="placeholder">
                </div>

                <div class="column is-half">
                    <section class="section">
                        <h1 class="title is-4">{{ product.name }}</h1>
                        
                        <p v-if="product.description">{{ product.description }}</p>
                        
                        <hr>
                        
                        <span class="tag is-rounded is-large is-danger is-light" v-if="!product.in_stock">
                          Sold out
                        </span>
                        
                        <span class="tag is-rounded is-large is-info is-light">
                          {{ product.price }}
                        </span>
                    </section>

                    <section class="section">
                      <form action="" @submit.prevent="add">
                        <ProductVariation 
                          v-for="(variations, type) in product.variations"
                          :type="type"
                          :variations="variations"
                          :key="type"
                          v-model="form.variation"
                        />

                        {{ form }}

                        <hr>

                        <div class="field has-addons" v-if="form.variation">
                          <div class="control">
                            <div class="select is-fullwidth is-medium">
                              <select name="" id="" v-model="form.quantity">
                                <option :value="x" v-for="x in parseInt(form.variation.stock_count)" :key="x">
                                  {{ x }}
                                </option>
                              </select>
                            </div>
                          </div>

                          <div class="control">
                            <button type="submit" class="button is-success is-medium is-fullwidth">Add to Cart</button>
                          </div>
                        </div>
                      </form>
                    </section>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
  import {mapActions} from 'vuex'
  import ProductVariation from '@/components/products/ProductVariation';

	export default {
    data () {
      return {
        product: null,
        form: {
          variation: '',
          quantity: 1
        }
      }
    },

    watch: {
      'form.variation' () {
        this.form.quantity = 1
      }
    },

    components: {
      ProductVariation
    },

    methods: {
      ...mapActions({
        store: 'cart/store'
      }),

      add() {
        this.store([{
          id: this.form.variation.id,
          quantity: this.form.quantity
        }])

        this.form = {
          variation:  '',
          quantity: 1
        }
      }
    },

    async asyncData ({ params, app }) {
      let response = await app.$axios.$get(`products/${params.slug}`)

      return {
        product: response.data
      }
    }
  }
</script>