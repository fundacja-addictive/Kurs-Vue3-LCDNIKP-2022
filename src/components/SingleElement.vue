<template>
    <button :disabled="block" :class="{'btn': true, 'btn-marked': isMarked, 'btn-miss': isMiss, 'btn-dead': isDead, 'btn-hit': isHit }" v-on:click="click()">{{ element.x + element.y }}</button>
</template>

<script>
import { v4 } from 'uuid';

    export default {
        data: function () {
            return {
                identifier: v4(),
                isMarked: false,
            };
        },
        props: {
            element: null,
            block: null,
            isMiss: null,
            isHit: null,
            isDead: null,
        },
        mounted () {
            console.log('Mounted!');
            if (this.element.isMarked)
                this.isMarked = true;
        },
        methods: {
            showLog: function (text) {
                var message = 'Klik: ';
                console.log(message + text);
            },
            click: function () {

                this.isMarked = !this.isMarked;

                // console.log(this.element.x);
                // hey elementClick happened with element
                this.$emit('elementClick', {
                    x: this.element.x,
                    y: this.element.y,
                    marked: this.isMarked,
                });
            },
        },
    }
</script>

<style>
    .btn {
        max-width: 100%;
        max-height: 100%;
        border: 1px solid black;
        margin: 1px;
        aspect-ratio: 1 / 1;
    }

    .btn-marked {
        background-color: deepskyblue;
    }
    
    .btn-miss {
        background-color: gray;
    }

    .btn-hit {
        background-color: red;
    }

    .btn-dead {
        background-color: black;
    }
</style>