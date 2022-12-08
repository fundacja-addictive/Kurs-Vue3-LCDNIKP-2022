<template>
  <div class="app">
    <single-map 
      class="map"
      v-on:elementClicked="elementClicked"></single-map>
    <ship-picker ref="shipPicker"></ship-picker>
    <br/>
    <br/>
  </div>
</template>

<script>
import SingleMap from './components/SingleMap.vue';
import ShipPickerVue from './components/ShipPicker.vue';

export default {
  name: 'App',
  components: {
    SingleMap: SingleMap,
    ShipPicker: ShipPickerVue,
  },
  data: function () {
    return {
      name: 'Karol',
    };
  },
  mounted: function () {
    // console.log(this.$refs.shipPicker);
  },
  methods: {
    elementClicked: function (element) {
      // console.log("Clicked:", element);
      this.$refs.shipPicker.elementClicked(element);
    }
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
