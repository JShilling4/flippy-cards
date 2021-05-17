<template>
    <button
        :class="['button', { hovering: isHovering && showHoverEffect }]"
        :style="styleObject"
        @mouseover="isHovering = true"
        @mouseleave="isHovering = false"
    >
        <slot></slot>
    </button>
</template>

<script>
export default {
    name: "AppButton",
    props: {
        showShadow: {
            type: Boolean,
            default: true,
        },
        showHoverEffect: {
            type: Boolean,
            default: true,
        },
        minWidth: {
            type: String,
            default: "18rem",
        }
    },
    data() {
        return {
            styleObject: {
                boxShadow: this.showShadow
                    ? "0 13px 15px -13px rgb(242, 95, 58)"
                    : "",
                minWidth: this.minWidth,
            },
            isHovering: false,
        };
    },
};
</script>

<style lang="scss" scoped>
.button {
    font-family: "Be Vietnam", sans-serif;
    padding: 1.4rem 3.4rem 1.6rem;
    border: none;
    border-radius: 3rem;
    outline: none;
    font-weight: 600;
    letter-spacing: 1px;
    color: rgba(255, 255, 255, 0.8);
    background-color: $red-bright;
    text-align: center;
    cursor: pointer;
    transition: background-color 0.3s, letter-spacing .3s;
    &.hovering {
        background-color: lighten($red-bright, 5%);
        letter-spacing: 1.4px;
    }
}
</style>