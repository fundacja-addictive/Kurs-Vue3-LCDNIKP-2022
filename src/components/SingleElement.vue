<template>
    <button class="btn" v-on:click="showLog(identifier)">{{ rowId }}</button>
</template>

<script>
import { v4 } from 'uuid';

    export default {
        data: function () {
            return {
                word: 'Pies',
                identifier: v4(),
            };
        },
        props: {
            rowId: null,
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
        },
    }
</script>

<style>
    .btn {
        width: 2em;
        height: 2em;
        border: 1px solid black;
        margin: 1px;
    }
</style>