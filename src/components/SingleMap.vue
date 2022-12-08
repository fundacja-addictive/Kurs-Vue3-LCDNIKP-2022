<template>
    <div class="map">
        <single-element 
            v-for="coordinate in coordinates" 
            v-bind:key="coordinate" 
            class="map-element" 
            :element="coordinate"
            v-on:elementClick="elementClicked"
        ></single-element>
    </div>
</template>

<script>
import SingleElement from './SingleElement.vue';

export default {
    name: 'SingleMap',
    components: {
        SingleElement: SingleElement,
    },
    data: function () {
        return {
            coordinates: [],
        };
    },
    props: {

    },
    mounted() {
        this.generateCoordinates();
    },
    methods: {
        generateCoordinates: function () {
            for (var x = 65; x < 75; x++) {
                for (var y = 1; y <= 10; y++) {
                    this.coordinates.push({
                        x: String.fromCharCode(x),
                        y: y,
                    });
                }
            }
        },
        elementClicked: function (element) {
            console.log(element);
            this.$emit('elementClicked', element);
        },
    }
};
</script>

<style>
.map {
  /* max-width: 100%;
  max-height: 100%; */
  display: grid;
  grid-template-columns: repeat(10, auto);
  grid-template-rows: repeat(10, auto);
}
</style>