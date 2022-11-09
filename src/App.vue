<template>
  <div>
    <div class="map">
      <div v-for="row in [1,2,3,4,5,6,7,8,9,10]" v-bind:key="row" class="map-row">
        <div v-for="column in [1,2,3,4,5,6,7,8,9,10]" v-bind:key="column" class="map-field">
          <single-element :rowId="row + '.' + column"></single-element>
        </div>
      </div>
    </div>
    <br/>
    <br/>
    <single-field></single-field>
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue';
import SingleElement from './components/SingleElement.vue';
import SingleField from './components/SingleField.vue';

// console.log(SingleElement);

export default {
  name: 'App',
  components: {
    // HelloWorld,
    SingleElement,
    SingleField,
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

.map-row, .map-column {
  display: inline-block;
}
</style>
