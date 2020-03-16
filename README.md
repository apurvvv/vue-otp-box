# vue-otp-box

> _Simple but amazing one-time password input component for the web as well as mobile with Vue 2.x._

## Installation

```
npm i vue-otp-box
```

## How to use it

```
<template>
  <div>
    //Example
    <vue-otp-box
      :numberOfBoxes="4"
      color="orange"
      position="center"
      @inputValue="inputValue"
    ></vue-otp-box>
  </div>
</template>

<script>
 import VueOtpBox from "vue-otp-box";
 import "vue-otp-box/dist/VueOtpBox.css";

export default {
  components: {
    VueOtpBox
  },

  data() {
    return {
      otp: undefined,
    };
  },

  methods: {
    inputValue(otp) {
      this.otp = otp;
    }
  }
};
</script>
```

## Props

| Prop Name     | Type   | Default Value  | Value                     |
| ------------- | ------ | -------------- | ------------------------- |
| numberOfBoxes | Number | 4              | 4 or 6 is recommended     |
| color         | String | java (#26a69a) | Any color will work       |
| position      | String | center         | "left", "right", "center" |

## Event

| Name        | Description                         |
| ----------- | ----------------------------------- |
| @inputValue | gets you the otp code of the input. |
