<template>
<transition name="slide-down">
    <div class="title-wrapper" v-show="ifTitleAndMenuShow">
        <div class="left">
            <span class="icon-back icon"></span>
        </div>
        <div class="right">
            <div class="icon-wrapper">
                <span class="icon-cart icon">买</span>
            </div>
            <div class="icon-wrapper">
                <span class="icon-person icon"></span>
            </div>
            <div class="icon-wrapper">
                <span class="icon-more icon"></span>
            </div>
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

.title-wrapper {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 101;
    width: 100%;
    height: px2rem(48);
    background: white;
    display: flex;
    box-shadow: 0 px2rem(5) px2rem(5) rgba(0, 0, 0, 0.15);

    .left {
        flex: 0 0 px2rem(50);
        @include center;

    }

    .right {
        display: flex;
        flex: 1;
        justify-content: flex-end;

        .icon-wrapper {
            flex: 0 0 px2rem(40);
            @include center;

            .icon-cart {
                font-size: px2rem(22);
            }

        }
    }

}
</style>
