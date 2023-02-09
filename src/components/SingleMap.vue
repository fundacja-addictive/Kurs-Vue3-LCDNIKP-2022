<template>
    <div class="map">
        <single-element 
            v-for="coordinate in coordinates" 
            v-bind:key="coordinate" 
            class="map-element" 
            :element="coordinate"
            v-on:elementClick="elementClicked"
            :block="blockPicking"
            :isMiss="coordinate.isMiss"
            :isHit="coordinate.isHit"
            :isDead="coordinate.isDead"
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
        blockPicking: null,
        overrideCoordinates: null,
    },
    mounted() {
        this.generateCoordinates();
    },
    methods: {
        generateCoordinates: function () {
            for (var x = 65; x < 75; x++) {
                for (var y = 1; y <= 10; y++) {
                    var override;
                    if (this.overrideCoordinates)
                        override = this.overrideCoordinates.find(c => {
                            return c.x == String.fromCharCode(x) && c.y == y;
                        });

                    this.coordinates.push({
                        x: String.fromCharCode(x),
                        y: y,
                        isMarked: override ? true : undefined,
                        isMiss: false,
                        isHit: false,
                        isDead: false,
                    });
                }
            }
        },
        elementClicked: function (element) {
            this.$emit('elementClicked', element);
        },
        missElement: function (coordinates) {
            this.coordinates.find(c => c.x == coordinates.x && c.y == coordinates.y).isMiss = true;
        },
        hitElement: function (coordinates) {
            this.coordinates.find(c => c.x == coordinates.x && c.y == coordinates.y).isHit = true;
        },
        killElement: function (coordinates) {
            this.coordinates.find(c => c.x == coordinates.x && c.y == coordinates.y).isDead = true;
        }
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