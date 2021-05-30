<template>
  <nav>
    <div class="menu-item"><a href="#">Regulamin</a></div>
    <div class="menu-item"><a href="#">O nas</a></div>
    <div class="menu-item"><a href="#">Kontakt</a></div>
    <Dropdown title="Services" :items="services" />
    
  </nav>
</template>

<script>
import Dropdown from './Dropdown';
export default {
  name: 'navbar',
  components: {
    Dropdown
  },
  data () {
    return {
      services: [
        {
          title: 'Spodenki',
          link: '#'
        },
        {
          title: 'Swetry',
          link:'#'
        },
        {
          title: 'Kurtki',
          link: '#'
        }
      ]
    }
  }
}
</script>

<style>


nav {
  align-items: left;
  justify-content: left;
}
nav .menu-item {
  display: flex;
  top:2px;
  color: rgb(44, 44, 44);
  background-color: rgb(54, 54, 54);
  padding: 10px 20px;
  position: relative;
  text-align: center;
  border-bottom: 3px solid transparent;
  transition: 0.4s;
  z-index: 1;
  border-bottom-color: #444444;
}
nav .menu-item.active,
nav .menu-item:hover {
  background-color: #444;
  border-bottom-color: #ffffff;
}
nav .menu-item a {
  color: rgb(255, 255, 255);
  text-decoration: none;
}
</style>