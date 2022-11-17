<template lang="">
    <div id="someDiv1" name="someDiv">
        <div class="border" style="font-size:2em">x</div>
        <div class="someStyle border">x</div>
        <div name="div1" class="someStyle border">x1</div>
        <div name="div1" class="someStyle border">x2</div>
        <div class="someStyle border">x</div>
        <div class="someStyle border">x</div>
    </div>
</template>
<script>
export default {
    
}
</script>
<style>
    #div1 {
        font-size: 2em;
    }

    .someStyle {
        background-color: green;
        font-size: 5px;
    }

    #someDiv1 {
        /* color: yellow; */
        border: 1px dotted blue;
    }

    .border {
        background-color: yellow;
        border: 1px solid purple;
        margin-top: 1px;
    }
</style>