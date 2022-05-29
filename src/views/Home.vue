<template>
  <div class="home">
    <section class="hero is-small is-black mb-4">  
    <img src="@/assets/ddd.png" class="img-fluid h-50" alt="...">
    </section>
    
 <br>
 <div class="app">
        <carousel
            @next="next"
            @prev="prev"
        >
            <carousel-slide v-for="(slide,index) in slides" 
            :key="slide" 
            :index="index"
            :visibleSlide="visibleSlide"
            :direction="direction"
            >
              <img :src="slide" />
            </carousel-slide>
            
                </carousel>
            </div>  
    <br>
    <br>
    <div class="columns is-centered">
      <div class="columns is-12">
        <h2 class="is-size-3 has-text-centered mb-5">Najnowsze produkty</h2>
      </div>
    </div>
    <div class="columns is-multiline">    
      <div 
      class="column is-3"
      v-for="product in latestProducts"
      v-bind:key="product.id"
      >
       
<div class="main">
<div class="container">    
    <div class="row">
        <div class="col-sm-4">
<div class="card">
  <div class="flip-card">
    <div class="flip-card-inner">
        <div class="flip-card-front">
    <div class="box">
          <figure class="image mb-4">
            <img v-bind:src="product.get_thumbnail">
          </figure>

          <h3 class="is-size-5">{{ product.name }}</h3>
          <p class="is-size-4 has-text-grey">PLN {{ product.price }}</p>

          <router-link v-bind:to="product.get_absolute_url" class="button is-dark mt-4">Więcej</router-link>
        </div>   
    </div>
    <div class="flip-card-back">
        <div class="box">
          <figure class="image mb-4">
            <img v-bind:src="product.get_image">
          </figure>

          <h3 class="is-size-5">{{ product.name }}</h3>
          <p class="is-size-4 has-text-grey">PLN {{ product.price }}</p>
        
          <router-link v-bind:to="product.get_absolute_url" class="button is-dark mt-4">Więcej</router-link>
          
        </div>   
  </div>
    
  </div>
  </div>
  </div>
  </div>  
  </div>
  </div>
  </div>
          
      <br>
      <br>
         <br>
      <br>



  </div>
    </div> 
  </div>


</template>

<script>
import axios from 'axios'
import Carousel from '@/components/Carousel'
import CarouselSlide from '@/components/CarouselSlide'
import ProductBox from '@/components/ProductBox'


export default {
  name: 'Home',
  data() {
    return {
      showMobileMenu: false,
      slides:[
                'https://picsum.photos/id/230/1200/400',
                'https://picsum.photos/id/231/1200/400',
                'https://picsum.photos/id/232/1200/400',

      ],
      visibleSlide: 1,
      direction: 'left',
      latestProducts: []
    }
  },
  components: {
    ProductBox,
    Carousel,
    CarouselSlide,
  },
  mounted() {
    this.getLatestProducts()

    document.title = 'Asiola Butik'
  },
  computed : {
      slidesLen() {
        return this.slides.length;
      }
  },
  methods: {
      next(){
        if(this.visibleSlide >= this.slidesLen - 1) {
            this.visibleSlide = 0;

        }else {
            this.visibleSlide++;
        }
        this.direction= "left"
      },
      prev(){
        if(this.visibleSlide <= 0) {
            this.visibleSlide = this.slidesLen - 1;

        }else {
            this.visibleSlide--;
        }
        this.direction= "right"
      },
    async getLatestProducts(){
      this.$store.commit('setIsLoading', true)

      await axios
        .get('/api/v1/latest-products/')
        .then(response => {
          this.latestProducts = response.data 
        })
        .catch(error =>{
          console.log(error)
        })

      this.$store.commit('setIsLoading', false)  
    }
  }
}
</script>

<style>


</style>
