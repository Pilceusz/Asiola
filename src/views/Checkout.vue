<template>
    <div class="page-checkout">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Podsumowanie</h1>
            </div>

            <div class="column is-12 box">
                <table class="table is-fullwidth">
                    <thead>
                        <tr>
                            <th>Produkt</th>
                            <th>Cena</th>
                            <th>Ilość</th>
                            <th>Suma</th>
                        </tr>
                    </thead>

                    <tbody>
                        <tr
                            v-for="item in cart.items"
                            v-bind:key="item.product.id"
                        >
                            <td>{{ item.product.name }}</td>
                            <td>PLN {{ item.product.price }}</td>
                            <td>{{ item.quantity }}</td>
                            <td>PLN {{ getItemTotal(item).toFixed(2) }}</td>
                        </tr>
                    </tbody>

                    <tfoot>
                        <tr>
                            <td colspan="2">Suma</td>
                            <td>{{ cartTotalLength }}</td>
                            <td>PLN {{ cartTotalPrice.toFixed(2) }}</td>
                        </tr>
                    </tfoot>
                </table>
            </div>

            <div class="column is-12 box">
                <h2 class="subtitle">Informacje do zamówienia</h2>

                <p class="has-text-grey mb-4">* Wszystkie pola muszą zostać wypełnione</p>

                <div class="columns is-multiline">
                    <div class="column is-6">
                        <div class="field">
                            <label>Imię*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="first_name">
                            </div>
                        </div>

                        <div class="field">
                            <label>Nazwisko*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="last_name">
                            </div>
                        </div>

                        <div class="field">
                            <label>E-mail*</label>
                            <div class="control">
                                <input type="email" class="input" v-model="email">
                            </div>
                        </div>

                        <div class="field">
                            <label>Telefon kontaktowy*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="phone">
                            </div>
                        </div>
                    </div>

                    <div class="column is-6">
                        <div class="field">
                            <label>Adres*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="address">
                            </div>
                        </div>

                        <div class="field">
                            <label>Kod-pocztowy*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="zipcode">
                            </div>
                        </div>

                        <div class="field">
                            <label>Miasto*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="place">
                            </div>
                        </div>

                        <div class="field">
                            <label>Rozmiar*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="size">
                            </div>
                        </div>

                        <br>
                         <div class="field">
                            <label> Metoda płatności* </label>
                            <select v-model="payment_type">
                                <option disabled value="">Proszę wybrać sposób dostawy</option>
                                <option>Przelew tradycyjny</option>
                                <option>Blik</option>
                                <option>Za pobraniem</option>
                            </select>
                            <span> Wybrano: {{ payment_type }} </span>
                        </div>



                            <br>
                        <div class="field">
                            <label> Sposób dostawy* </label>
                            <select v-model="trip">
                                <option disabled value="">Proszę wybrać sposób dostawy</option>
                                <option>INPOST 48</option>
                                <option>Odbiór osobisty</option>
                                <option>Paczkomat</option>
                            </select>
                            <span> Wybrano: {{ trip }} </span>
                        </div>



                    </div>
                </div>

                <div class="notification is-danger mt-4" v-if="errors.length">
                    <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                </div>

                <hr>

                <div id="card-element" class="mb-5"></div>

                <template v-if="cartTotalLength">
                    <hr>

                    <button class="button is-dark" @click="submitForm">Przejdź do płatności</button>
                </template>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'Checkout',
    data() {
        return {
            cart: {
                items: []
            },
            stripe: {},
            card: {},
            first_name: '',
            last_name: '',
            email: '',
            phone: '',
            address: '',
            zipcode: '',
            place: '',
            size: '',
            payment_type: '',
            trip: '',
            errors: []
        }
    },
    mounted() {
        document.title = 'Zamówienie | Asiola Butik'

        this.cart = this.$store.state.cart
        
        if (this.cartTotalLength > 0) {
            this.stripe = Stripe('pk_test_51ImOgWIcogfCGBZIumpr9o1QfzZwihd0CWoyWNKMK1QZg9CfcCCod9ujgT4mFyygFrVhnzRDOnsrUo4l2aSgDOQ400HYmUsz6K')
            const elements = this.stripe.elements();
            this.card = elements.create('card', { hidePostalCode: true })
            this.card.mount('#card-element')
        }

    },
    methods: {
       
        getItemTotal(item) {
            return item.quantity * item.product.price
        },
        submitForm() {
            this.errors = []
            if (this.first_name === '') {
                this.errors.push('Nie uzupełniono pola Imię!')
            }
            if (this.last_name === '') {
                this.errors.push('Nie uzupełniono pola Nazwisko!')
            }
            if (this.email === '') {
                this.errors.push('Nie uzupełniono pola E-mail!')
            }
            if (this.phone === '') {
                this.errors.push('Nie uzupełniono pola Telefon kontaktowy!')
            }
            if (this.address === '') {
                this.errors.push('Nie uzupełniono pola Adres!')
            }
            if (this.zipcode === '') {
                this.errors.push('Nie uzupełniono pola Kod-pocztowy!')
            }
            if (this.place === '') {
                this.errors.push('Nie uzupełniono pola Miasto!')
            }
             if (this.size === '') {
                this.errors.push('Nie uzupełniono pola Rozmiar!')
            }
             if (this.payment_type === '') {
                this.errors.push('Nie uzupełniono pola Metoda Płatności!')
            }
            if (!this.errors.length) {
                this.$store.commit('setIsLoading', true)
                this.stripe.createToken(this.card).then(result => {                    
                    if (result.error) {
                        this.$store.commit('setIsLoading', false)
                        this.errors.push('Coś poszło nie tak. Proszę spróbować ponownie')
                        console.log(result.error.message)
                    } else {
                        this.stripeTokenHandler(result.token)
                    }
                })
            }
        },
        async stripeTokenHandler(token) {
            const items = []
            for (let i = 0; i < this.cart.items.length; i++) {
                const item = this.cart.items[i]
                const obj = {
                    product: item.product.id,
                    quantity: item.quantity,
                    price: item.product.price * item.quantity
                }
                items.push(obj)
            }
            const data = {
                'first_name': this.first_name,
                'last_name': this.last_name,
                'email': this.email,
                'address': this.address,
                'zipcode': this.zipcode,
                'place': this.place,
                'phone': this.phone,
                'size': this.size,
                'trip': '',
                'payment_type':'this.payment_type',
                'items': items,
                'stripe_token': token.id
            }
            await axios
                .post('/api/v1/checkout/', data)
                .then(response => {
                    this.$store.commit('clearCart')
                    this.$router.push('/cart/success')
                })
                .catch(error => {
                    this.errors.push('Something went wrong. Please try again')
                    console.log(error)
                })
                this.$store.commit('setIsLoading', false)
        }
    },
    computed: {
        cartTotalPrice() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.product.price * curVal.quantity
            }, 0)
        },
        cartTotalLength() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        }
    }
}
</script>