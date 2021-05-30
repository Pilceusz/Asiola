<template>
 <div class="contact">
<h2 class="is-size-2 has-text-left">Skontaktuj się z nami</h2>
    <!--Section description-->
   <h4 class="is-size-4 has-text-left">Masz jakieś pytania? Potrzebujesz pomocy ? Napisz do nas wiadomość ,a odpiszemy najszybciej jak to możliwe</h4>
<br>
<div class="con colums-is-multiline">
 <div class="col-md-2 text-center colums-is-multiline">
           
              <p><i class="fas fa-map-marker-alt fa-3x" ></i></p>
                   <p> Tczew, ul Reymonta 23, Polska </p>
                

              <p><i class="fas fa-phone mt-4 fa-3x" ></i></p>
                     <p> + 01 234 567 89</p>
                

              <p><i class="fas fa-envelope mt-4 fa-3x"></i></p>
                     <p> Asiolabutik@wpl.pl</p>
                
           
        </div>
</div>
 </div>
</template>












<script>
import axios from 'axios'
export default {
    data() {
      return {
        fields: {
          name: "",
          email: "",
          subject: "",
          message: ""
        }
      
      };
    },
    methods: {
      submitForm(event) {
        event.target.classList.add("was-validated");
        // submit form
      }
    }
  };
</script>


  <style>

</style>