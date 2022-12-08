<template lang="">
    <div>
        <single-ship 
            v-for="ship in ships"
            v-bind:key="ship.id"
            :ship="ship"
        ></single-ship>
    </div>
</template>
<script>
import SingleShipVue from './SingleShip.vue';


export default {
    components: {
        SingleShip: SingleShipVue,
    },
    data: function () {
        return {
            markedFields: [],
            ships: [
                {
                    id: 1,
                    length: 4,
                    state: 'free', // free, current, picked
                },
                {
                    id: 2,
                    length: 3,
                    state: 'free', // free, current, picked
                },
                {
                    id: 3,
                    length: 3,
                    state: 'free', // free, current, picked
                },
                {
                    id: 4,
                    length: 2,
                    state: 'free', // free, current, picked
                },
                {
                    id: 5,
                    length: 2,
                    state: 'free', // free, current, picked
                },
                {
                    id: 6,
                    length: 2,
                    state: 'free', // free, current, picked
                },
                {
                    id: 7,
                    length: 1,
                    state: 'free', // free, current, picked
                },
                {
                    id: 8,
                    length: 1,
                    state: 'free', // free, current, picked
                },
                {
                    id: 9,
                    length: 1,
                    state: 'free', // free, current, picked
                },
                {
                    id: 10,
                    length: 1,
                    state: 'free', // free, current, picked
                },
            ]
        }
    },
    mounted: function () {
        this.ships[0].state = 'current';
    },
    methods: {
        elementClicked: function (element) {
            console.log("Clicked AGAIN", element);
            if (element.marked)
                this.markedFields.push(element);
        },  
    }
}
</script>
<style>
    
</style>