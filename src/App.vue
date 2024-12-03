<template>
  <div class="login-title">
  <h1>
    <span class="icon">🗑️</span> Login with <span class="highlight">Garbage</span>


  </h1>
</div>

  <HelloWorld msg="Welcome to Your Vue.js App"/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.login-title {
  text-align: center;
  font-family: 'Arial', sans-serif;
  margin: 20px 0;
  color: #333;
}

.login-title h1 {
  font-size: 2.5rem;
  font-weight: bold;
  position: relative;
  display: inline-block;
}

.icon {
  font-size: 3rem;
  vertical-align: middle;
  margin-right: 8px;
}

.highlight {
  color: #e6bb39;
  background: #54bb592a;
  padding: 0 8px;
  border-radius: 5px;
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.1);
  display: inline-block;
  transform: rotate(-15deg); 
}


</style>
