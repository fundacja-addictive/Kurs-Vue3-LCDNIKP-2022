<template lang="">
    <div>
        <div
            :class="shipClasses"
            v-for="node in ship.length"
            v-bind:key="node"
        >
        </div>
    </div>
</template>
<script>
export default {
  data: function () {
    return {

    };
  }, 
  props: {
    ship: Object
  },
  mounted: function () {

  },
  computed: {
    shipClasses: function () {
      var classes = "shipNode ";

      if (this.ship.state == 'current') 
        classes += "currentShip ";

      if (this.ship.state == 'picked')
        classes += "pickedShip ";

      if (this.ship.state == 'dead')
        classes += 'deadShip ';

      return classes;
    },
  },
}
</script>
<style scoped>
    .shipNode {
        background: gray;
        border: 1px solid black;
        width: 1em;
        height: 1em;
        display: inline-block;
    }

    .currentShip {
      background-color: blue;
    }

    .pickedShip {
      background-color: green;
    }

    .deadShip {
      background-color: black;
    }
</style>