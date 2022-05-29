<template>
  <div id="wrapper">
    <nav class="navbar is-dark">
      <div class="navbar-brand">
      <router-link to="/" class="navbar-item"><strong>Strona główna</strong></router-link>
    

      <a class="navbar-burger" aria-label="menu" aria-expanded="false" data-target="navbar-menu" @click="showMobileMenu = !showMobileMenu">
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
      </a>
      </div>
    <div class="navbar-menu" id="navbar-menu" v-bind:class="{'is-active': showMobileMenu}">
      <div class="navbar-start">
          <div class="navbar-item">
            <form method="get" action="/search">
              <div class="field has-addons">
                <div class="control">
                  <input type="text" class="input" placeholder="Wyszukaj" name="query">
                </div>

                <div class="control">
                  <button class="button is-dark">
                      <span class="icon">
                      <i class="fas fa-search"></i>
                      </span>
                  </button>
                </div>
              </div>
            </form>
          </div>
        </div>
      <div class="navbar-end">
      <router-link to="/regulamin" class="navbar-item">Regulamin</router-link>
      <router-link to="/about" class="navbar-item">O nas</router-link>
      <router-link to="/kontakt" class="navbar-item">Kontakt</router-link>
      <Dropdown title="Produkty" :items="services" />

        <div class="navbar-item">
          <div class="buttons">
            <template v-if="$store.state.isAuthenticated">
                <router-link to="/my-account" class="navbar-item">Moje konto</router-link>
              </template>
            <template v-else>
             <router-link to="/log-in" class="button is-dark">
             <span class="icon"><i class="fas fa-user"></i></span>
             <span><strong>Zaloguj się</strong></span>
             </router-link>
            </template>

             <router-link to="/cart" class="button is-dark">
                <span class="icon"><i class="fas fa-shopping-cart"></i></span>
                <span><strong>Koszyk ({{ cartTotalLength }})</strong></span>
             </router-link>
          </div>
        </div>
       </div>
      </div>   
    </nav>

    <div class="is-loading-bar has-text-centered" v-bind:class="{'is-loading': $store.state.isLoading }">
      <div class="lds-dual-ring"></div>
    </div>

    <section class="section">
   <router-view/>
    </section>

    <footer class="footer">
     <form class="form-multiline has-text-centered">Znajdź nas na :
     <a href="https://www.facebook.com/Asiola-Butik-102804948598917" class="fa fa-facebook"></a>
     <a href="https://www.instagram.com/" class="fa fa-instagram"></a>
     </form>
      <br>
      <p class="has-text-centered">Copyright (c) 2021</p>
      
    </footer>  
  </div>
</template>

<script>
import axios from 'axios'
import Navbar from './components/Navbar';
import Dropdown from './components/Dropdown';

export default {
  name: 'navbar',
  components: {
    Dropdown
  
  },
  data() {
    return {
      services: [
        {
          title: 'Spodenki',
          link: '/summer'
        },
        {
          title: 'Swetry',
          link:'/swetry'
        },
        {
          title: 'Kurtki',
          link: '/kurtki'
        }
      ],
      showMobileMenu: false,
      cart: {
        items: []
      }
    }
  },
  beforeCreate() {
    this.$store.commit('initializeStore')

        const token = this.$store.state.token
    if (token) {
        axios.defaults.headers.common['Authorization'] = "Token " + token
    } else {
        axios.defaults.headers.common['Authorization'] = ""
    }
  },
  mounted() {
    this.cart = this.$store.state.cart
  },
  computed: {
      cartTotalLength() {
          let totalLength = 0

          for (let i = 0; i < this.cart.items.length; i++) {
              totalLength += this.cart.items[i].quantity
          }

          return totalLength
      }
  }
}
</script>

<style lang="scss">
@import '../node_modules/bulma';
@import url('https://fonts.googleapis.com/css2?family=Marck+Script&display=swap');

.lds-dual-ring {
  display: inline-block;
  width: 80px;
  height: 80px;
}
.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 6px solid rgb(66, 66, 66);
  border-color: rgb(73, 73, 73) transparent rgb(102, 102, 102) transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@media screen and (max-height: 450px) {
  .sidenav {padding-top: 15px;}
  .sidenav a {font-size: 18px;}
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
.is-loading-bar {
  height: 0;
  overflow: hidden;
  -webkit-transition: all 0.3s;
  transition: all 0.3s;
  &.is-loading {
    height: 80px;
  }
}
header {
  width: 100vw;
  background-color: rgb(0, 0, 0);
  padding: 15px;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
footer {
  background-color: #f2f2f2;
  padding: 25px;
}

.fa {
  color: black;
  padding: 5px;
  font-size: 30px;
  text-align: center;
  text-decoration: none;
}
.fa:hover {
  opacity: 0.7;
}

</style>
