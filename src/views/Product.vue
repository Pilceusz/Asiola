

<template>
<div class="page-product">
        <div class="columns is-multiline">
              <div class="wrapper row">
                <div class="preview col-md-5">

                    <div class="preview-pic tab-content">
                      <div class="tab-pane active" id="pic-1"><img v-bind:src="product.get_thumbnail"></div>
                      <div class="tab-pane" id="pic-2"><img v-bind:src="product.get_thumbnail"></div>

                    </div>
                    <ul class="preview-thumbnail nav nav-tabs">
                      <li class="active"><a data-target="#pic-1" data-toggle="tab"><img v-bind:src="product.get_thumbnail"></a></li>
                      <li><a data-target="#pic-2" data-toggle="tab"><img v-bind:src="product.get_thumbnail"></a></li>

                    </ul>

                </div>





            <div class="column is-3">
                 <h1 class="title">{{ product.name }}</h1>

                <h2 class="subtitle">Opis</h2>

                <p>{{ product.description }}</p>

                <p><strong>Cena: </strong>PLN {{ product.price }}</p>

                <p><strong>Rozmiar:</strong></p>
                    <select v-model="selected">
                       <option disabled value="">Proszę wybrać rozmiar</option>
                       <option>S</option>
                       <option>M</option>
                       <option>L</option>
                       <option>Uni</option>
                    </select>
                    <span> Wybrano: {{ selected }} </span>

                <div class="field has-addons mt-6">
                    <div class="control">
                            <input type="number" class="input" min="1" v-model="quantity">
                    </div>


                    <div class="control">
                        <a class="button is-dark" @click="addToCart()">Dodaj do koszyka</a>
                    </div>
                </div>
                </div>
            </div>
        </div>
      </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

export default {
    name: 'Product',
    data() {
        return {
            product: {},
            quantity: 1,
            selected: '',
        }
    },
    mounted() {
        this.getProduct()
    },

    methods: {
        async getProduct(){
            this.$store.commit('setIsLoading', true)

            const category_slug = this.$route.params.category_slug
            const product_slug = this.$route.params.product_slug

            await axios
                .get(`/api/v1/products/${category_slug}/${product_slug}`)
                .then(response => {
                    this.product = response.data

                    document.title = this.product.name + ' | Asiola Butika'
                })
                .catch(error => {
                    console.log(error)
                })

            this.$store.commit('setIsLoading', false)
        },


        addToCart() {
            if (isNaN(this.quantity) || this.quantity < 1) {
                this.quantity = 1
            }
            const item = {
                product: this.product,
                quantity: this.quantity,
                selected: this.selected
            }
            this.$store.commit('addToCart', item)
            toast({
                message: 'Produkt został dodany do koszyka',
                type: 'is-dark',
                dismissible: true,
                pauseOnHover: true,
                duration: 2000,
                position: 'bottom-right',
            })
        }
    }
}
</script>
