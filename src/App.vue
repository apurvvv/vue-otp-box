<template>
    <div id="app">
        <vue-otp-box :numberOfBoxes="4" @inputValue="emitInput" />
    </div>
</template>

<script>
    import VueOtpBox from "./components/VueOtpBox.vue";

    export default {
        name: "App",
        components: {
            VueOtpBox
        },
        data() {
            return {
                otp: undefined
            };
        },
        methods: {
            emitInput(value) {
                this.otp = value;
            }
        }
    };
</script>
