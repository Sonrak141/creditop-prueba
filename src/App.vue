<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <h2>Calcula tu prestamo con nosotros de una manera rapida y sencilla</h2>
  <CalculadoraPrestamo />
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import CalculadoraPrestamo from "./components/CalculadoraPrestamo.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
    CalculadoraPrestamo,
  },
};
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
</style>
