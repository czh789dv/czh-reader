<template>
<transition name="slide-up">
    <div class="menu-wrapper" v-show="ifTitleAndMenuShow">
        <div class="icon-wrapper">
            <span class="icon-menu icon"></span>
        </div>
        <div class="icon-wrapper">
            <span class="icon-progress icon"></span>
        </div>
        <div class="icon-wrapper">
            <span class="icon-bright icon"></span>
        </div>
        <div class="icon-wrapper">
            <span class="icon-a icon">A</span>
        </div>
    </div>
</transition>
</template>

<script>
export default {
    props: {
        ifTitleAndMenuShow:{
            type:Boolean,
            default:false
        }
    }

}
</script>

<style lang="scss" scoped>
@import '../assets/styles/global.scss';

.menu-wrapper {
    position: absolute;
    bottom: 0;
    left: 0;
    z-index: 101;
    width: 100%;
    height: px2rem(48);
    background: white;
    display: flex;
    box-shadow: 0 px2rem(-5) px2rem(5) rgba(0, 0, 0, 0.15);

    .icon-wrapper {
        flex: 1;
        @include center;

        .icon-progress {
            font-size: px2rem(24);
        }

        .icon-bright {
            font-size: px2rem(24);
        }
    }

}
</style>
