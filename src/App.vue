<template>
    <div id="app">
        <VueOtp :numberOfBoxes="5" position="left" color="#fe4a49" />
    </div>
</template>

<script>
    import VueOtp from "./VueOtp";
    export default {
        name: "App",
        components: {
            VueOtp
        }
    };
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        width: 100%;
        color: #2c3e50;
    }
</style>
