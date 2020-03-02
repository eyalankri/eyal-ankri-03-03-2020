<template>
<div>
  <Navbar />
      <div class="container" style="padding:100px 0">
<router-view/> 
      </div>
            
   <Footer />
</div>
</template>

<script>
  import Footer from "@/components/layout/footer";
  import Navbar from "@/components/layout/navbar";
  import InfoBar from "@/components/layout/infobar";


  export default {
    name: "App",
    components: {
      Navbar,
      InfoBar,
      Footer
    }
  };
</script>

<style>
   @import './assets/style/main.css';
</style>
