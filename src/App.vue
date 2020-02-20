
<template><vn-alert> 
  <div>Case contains sensitive health information</div>
</vn-alert></template>
<template>
  
</template>

<script>
import VnAlert from './components/VnAlert.vue'
import VnClosablePanel from './components/VnClosablePanel.vue'

export default {
  name: 'app', 
  components: {
    VnAlert
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
