<template>
  <div id="app">
    <Navbar />
    <div class="container">
      <router-view/>
    </div>
  </div>
</template>

<script>
//  Imported Navbar component
import Navbar from './components/Navbar'
export default {
  name: 'App',
  components: {
    //Register Navbar component
    Navbar
  }
}
</script>
