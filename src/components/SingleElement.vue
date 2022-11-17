<template>
    <button :class="'btn ' + (isMarked ? 'btn-marked' : '')" v-on:click="click()">{{ element.x + element.y }}</button>
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
        },
        mounted () {
            console.log('Mounted!');
            // setInterval(() => {
            //     this.word = this.word + '.';
            // }, 1000);
            // this.showLog("Pies ");
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
                this.$emit('elementClick', this.element);
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
</style>