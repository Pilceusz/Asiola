<template>
    <div class="carousel">
        <slot></slot>


        <button @click="next" class="next fa fa-arrow-right"></button>
        <button @click="prev" class="prev fa fa-arrow-left"></button>
    </div>
</template>




<script>
export default {
    data()  {
        return {

        }
        
    },
    methods: {
        next() {
            this.$emit('next')
        },
        prev() {
            this.$emit('prev')
        },
    }
}
</script>








<style >
    .carousel{
        display: block;
        position: fixed;
        width: auto;
        height: 350px;
        overflow: hidden;

    }

    button:focus, button:hover{
        outline: none;
        cursor: pointer;

    }
    .next {
        right: 0;
        position: absolute;
        height: 40px;
        width: 50px;
        top: calc(50% - 20px);
        background-color: rgba(0,0,0,0.8);
        border: none;
        color: white;
    }
    .prev {
        left: 0;
        position: absolute;
        height: 40px;
        width: 50px;
        top: calc(50% - 20px);
        background-color: rgba(0,0,0,0.8);
        border: none;
        color: white;
    }
</style>