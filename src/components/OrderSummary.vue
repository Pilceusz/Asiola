<template>
    <div class="box mb-4">
        <h3 class="is-size-4 mb-6">Zamówienie #{{ order.id }}</h3>

        <h4 class="is-size-5">Produkty</h4>

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
                    v-for="item in order.items"
                    v-bind:key="item.product.id"
                >
                    <td>{{ item.product.name }}</td>

                    <td>PLN {{ item.product.price }}</td>
                    <td>{{ item.quantity }}</td>
                    <td>PLN {{ item.product.price * item.quantity }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: 'OrderSummary',


    props: {
        order: Object
    },
    methods: {
        getItemTotal(item) {
            return item.quantity * item.product.price * item.selected
        },
        orderTotalLength(order) {
            return order.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        },
    }
}
</script>