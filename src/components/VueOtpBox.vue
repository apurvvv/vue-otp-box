<template>
    <div id="wrapper" :style="{ justifyContent: direction }">
        <template v-for="(char, index) in arraySize">
            <input
                class="inputBox"
                type="tel"
                :key="index"
                maxlength="1"
                v-model="arraySize[index]"
                v-on="listeners"
                @keydown="
                    handleEnterKey($event);
                    handleKeyDown($event, index);
                "
                @input="handleInput($event, index)"
                @paste="onPaste"
                :autofocus="index === 0"
                :ref="`otp${index}`"
                @focus="newColor(index)"
                @blur="defaultColor(index)"
            />
        </template>
    </div>
</template>
<script>
    require("@/assets/css/main.css");
    export default {
        name: "VueOtp",
        props: ["numberOfBoxes", "position", "color"],

        data() {
            return {
                arraySize: undefined,
                boxLength: undefined,
                direction: undefined,
                boxColor: "#26a69a"
            };
        },
        mounted() {
            this.handleBoxes();

            if (typeof this.color !== "undefined" && this.color) {
                this.boxColor = this.color;
            }

            this.handleAlignment();
        },
        methods: {
            handleBoxes() {
                if (
                    typeof this.numberOfBoxes === "undefined" &&
                    !this.numberOfBoxes
                ) {
                    this.boxLength = 4;
                    this.arraySize = Array(this.boxLength).fill("");
                } else if (typeof this.numberOfBoxes === "number") {
                    this.boxLength = this.numberOfBoxes;
                    this.arraySize = Array(this.boxLength).fill("");
                } else {
                    this.boxLength = 4;
                    this.arraySize = Array(this.boxLength).fill("");
                }
            },
            handleAlignment() {
                switch (this.position) {
                    case "left":
                        this.direction = "flex-start";

                        break;
                    case "right":
                        this.direction = "flex-end";
                        break;
                    case "center":
                        this.direction = "center";
                        break;
                    default:
                        this.direction = "center";
                }
            },
            newColor(index) {
                const i = "otp" + index;
                this.$refs[
                    i
                ][0].style.boxShadow = ` 0 0 5px  ${this.boxColor} inset`;
                this.$refs[i][0].style.border = `1px solid ${this.boxColor}`;
            },
            defaultColor(index) {
                const i = "otp" + index;
                this.$refs[i][0].style.boxShadow = " 0 0 5px #ccc inset";
                this.$refs[i][0].style.border = "solid 1px #ccc";
            },
            handleEnterKey(event) {
                if (event.key === "Enter") {
                    this.$emit("enter");
                    event.stopPropagation();
                }
            },
            sanitizeKeyData(key) {
                return key === "Unidentified" ? undefined : key;
            },
            emitInput() {
                const result = this.arraySize
                    .join("")
                    .slice(0, this.numberOfBoxes);
                this.$emit("inputValue", result);
            },
            handleKeyDown(event, index) {
                const key = this.sanitizeKeyData(event.key);
                if (!key) {
                    return;
                }
                if (key === "Backspace") {
                    if (this.arraySize[index]) {
                        return (this.arraySize[index] = "");
                    }

                    if (index > 0) {
                        event.target.previousElementSibling.focus();
                    }
                } else if (
                    !event.shiftKey &&
                    (key === "ArrowRight" || key === "Right")
                ) {
                    if (index < this.arraySize.length - 1) {
                        event.target.nextElementSibling.focus();
                    }
                } else if (
                    !event.shiftKey &&
                    (key === "ArrowLeft" || key === "Left")
                ) {
                    if (index > 0) {
                        event.target.previousElementSibling.focus();
                    }
                } else if (key.length === 1 && this.arraySize[index]) {
                    this.arraySize[index] = key;
                    this.$forceUpdate();
                    if (index < this.arraySize.length - 1) {
                        event.target.nextElementSibling.focus();
                    }
                    this.emitInput();
                }
            },
            handleInput(event, index) {
                const value = this.arraySize[index];

                if (value) {
                    if (value.length > 1) {
                        this.arraySize[index] = value[value.length - 1];
                    }
                    if (index < this.arraySize.length - 1) {
                        event.target.nextElementSibling.focus();
                    }
                }

                this.emitInput();
            },
            onPaste(event) {
                const clipboardData =
                    event.clipboardData || window.clipboardData;
                if (!clipboardData) {
                    return;
                }
                event.preventDefault();
                const code =
                    clipboardData.getData("Text") ||
                    clipboardData.getData("text/plain");
                this.fillCode(code);
            },
            fillCode(code) {
                code = code.trim();
                code = code.slice(0, this.boxLength);
                const parts = code.split("");
                parts.length = this.boxLength;
                this.arraySize = parts;
                const last = code.length - 1;
                setTimeout(() => {
                    this.arraySize[last] =
                        this.arraySize[last] &&
                        this.arraySize[last].slice(0, 1);
                    this.$forceUpdate();
                    this.$refs["otp" + (this.arraySize.length - 1)][0].focus();
                }, 0);
            }
        },
        computed: {
            listeners() {
                return {
                    ...this.$listeners
                };
            }
        }
    };
</script>
