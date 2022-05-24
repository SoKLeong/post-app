<template>
    <header class="justify-content-between">
        <button :class="[sideNavView ? 'animate' : 'revAnimate', 'btn btn-white d-xl-none sideNavButton']" @click="$emit('toggle-side-nav')">
            <i v-show="!sideNavView" class="fa fa-bars"></i>
            <i v-show="sideNavView" class="fa fa-close"></i>
        </button>
        <router-link to="/"><img src="@/assets/logo.png" alt="" /></router-link>
        <button @click="$emit('toggle-to-do')" class="btn btn-outline-primary text-primary d-xl-none"><i class="fa fa-check-square"></i> <span class="d-none d-sm-inline">TO-DO</span></button>
    </header>
</template>

<script>
export default {
    name: "Header",
    emits: ['toggle-side-nav', 'toggle-to-do'],
    props: {
        sideNavView: Boolean
    }
}
</script>

<style lang="scss" scoped>
@import "@/scss/_variables.scss";

header {
    padding: 16px 36px;
    background-color: white;
    display: flex;
    box-shadow: 0 1px 4px #00000019;
    position: sticky;
    top: 0;
    z-index: 5;

    img {
        width: auto;
        height: 32px;
    }

    .sideNavButton {
        transition: transform 0.2s;
        box-shadow: 0 0px 5px #aaa;

        &:focus {
            outline: 0;
        }

        &.animate {
            transform: rotate(0deg);
        }

        &.revAnimate {
            transform: rotate(180deg);
        }
    }

    .btn-outline-primary {
        font-size: 14px;
    }
}
</style>