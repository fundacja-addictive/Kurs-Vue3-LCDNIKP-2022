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
            markedFields: null,
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

        this.markedFields = [];

    },
    methods: {
        elementClicked: function (element) {
            if (element.marked == true)
                this.markedFields.push(element);
            else {
                var e = this.markedFields.findIndex(function (elem) {
                    return elem.x == element.x && elem.y == element.y;
                });
                this.markedFields.splice(e, 1);
            }
        },
        findCurrentShip: function () {
            return this.ships.find(s => {
                return s.state == 'current';
            });
        },
        setNextCurrent: function () {
            var free = this.ships.find(s => s.state == 'free');

            if (!free)
                return false;

            free.state = 'current';
            
            return true;
        },
        validateShip: function (shipArray) {
            var curr = this.findCurrentShip();

            if (!curr)
                return this.$emit('allShipsPicked', this.ships);

            if (curr.length == shipArray.length) {
                curr.state = 'picked';
                curr.nodes = shipArray;
                this.markedFields = [];

                this.setNextCurrent();
            }
        },  
    },
    watch: {
        markedFields: {
            deep: true,
            handler: function (fields) {
                var nodes = [];

                // U, D, L, R
                var findField = (fieldsToSearch, currentField, direction) => {
                    var checkX = currentField.x.charCodeAt(0) 
                        if (direction == 'L') 
                            checkX = checkX - 1;
                        if (direction == 'R')
                            checkX = checkX + 1;
                        checkX = String.fromCharCode(checkX);
                    var checkY = currentField.y + 0;
                        if(direction == 'U')
                            checkY += 1;
                        if(direction == 'D')
                            checkY -= 1;

                    console.log(checkX, checkY);

                    var found = fieldsToSearch.find((e) => {
                        return e.x == checkX && e.y == checkY;
                    });

                    console.log(found);

                    return found;
                };

                var checkOther = (field /* object */, directions /* array */) => {
                    var found = nodes.find(e => e.x == field.x && e.y == field.y);
                    if (found)
                        return [];

                    nodes.push(field);

                    directions.forEach(d => {
                        var result = findField(fields, field, d);

                        if (result) {
                            if (d == "U" || d == "D") {
                                return [...checkOther(result, ["U", "D"]), result];
                            }
                            if (d == "L" || d == "R")
                                return [...checkOther(result, ["L", "R"]), result];
                        } else {
                            return [];
                        }
                    });

                    return [];
                };

                if (fields.length > 0)
                    checkOther(fields[0], [
                        "U", 
                        "D", 
                        "L", 
                        "R"
                    ]);

                this.validateShip(nodes);
            },
        }
    }
}
</script>
<style>
    
</style>