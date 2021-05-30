<template>
    <div class="page-success">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Dziękujemy za złożenie zamówienia</h1>

                <p>Twoje zamówienie będzie dostarczone w jak najszybszym terminie !</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Success',
    mounted() {
        document.title = 'Podsumowanie | Asiola Butik'
    },
}
</script>