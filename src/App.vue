<template>
  <div class="app">
    <single-map class="map"></single-map>
    <br/>
    <br/>
    <!-- <single-field></single-field> -->
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue';
// import SingleElement from './components/SingleElement.vue';
// import SingleField from './components/SingleField.vue';
import SingleMap from './components/SingleMap.vue';

{

  // // integer
  // var i = 5;
  
  // // float
  // var fl1 = 5.5;
  // var fl2 = 5.0;
  // var fl3 = 0.0;
  
  // // string 
  // var s = "aaaa";
  
  // // object
  // var o1 = {a: 1, b: "aaa", c: {} };
  // var o2 = {};
  // var o3 = {a: {}, b: {}, x: function(){} };
  // o3.x();
  // // array
  // var a1 = [1, 2, 3];
  // var a2 = [];
  // var a3 = ["aaaa", 2, {}, function (){}];
  // a3[3]();
  
  // // function
  // var f1 = function () {};
  // var f2 = () => {};
  // var f3 = () => f2();
}

export default {
  name: 'App',
  components: {
    // HelloWorld,
    // SingleElement,
    // SingleField,
    SingleMap: SingleMap,
  },
  data: function () {
    return {
      name: 'Karol',
    };
  },
  mounted: function () {

  },
  methods: {
    
  },
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
/* 
.map-row, .map-column {
  display: inline-block;
} */

.map {
  width: 500px;
}

/* .app {
  width: 100%;
  height: 100%;
} */

</style>
