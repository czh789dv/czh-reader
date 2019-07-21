<template>
<div class="book">
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

    <div class="read-wrapper">
        <div id="read"></div>
        <div class="mask">
            <div class="left" @click="prevPage()"></div>
            <div class="center" @click="toggleTitleAndMenu"></div>
            <div class="right" @click="nextPage()"></div>
        </div>
    </div>
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
</div>
</template>

<script>
//导入epubjs库
import Epub from 'epubjs'
//传递一个书的路径 后期会放在服务器
const DOWNLOAD_URL = '/static/三体.epub'

// global.ePub = Epub
//全局化

export default {
    data() {
        return {
            ifTitleAndMenuShow: false
        }
    },
    methods: {
        toggleTitleAndMenu() {
            this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow
        },
        //上一页
        prevPage() {
            //调用Rendition.prev方法
            if (this.rendition) {
                this.rendition.prev()
            }

        },
        //下一页
        nextPage() {
            //调用Rendition.next方法
            if (this.rendition) {
                this.rendition.next()
            }

        },
        //渲染电子书
        showEpub() {
            //生成BOOK实例对象
            this.book = new Epub(DOWNLOAD_URL)
            console.log(this.book)
            //生成rendition
            this.rendition = this.book.renderTo('read', {
                //让书的宽高和屏幕一样
                width: window.innerWidth,
                height: window.innerHeight
            })
            //通过rendition.display渲染电子书
            this.rendition.display()
        }
    },
    mounted() {
        //生命周期钩子函数 
        this.showEpub()
    },

}
</script>

<style lang="scss" scoped>
@import 'assets/styles/global.scss';

.book {
    position: relative;

    .read-wrapper {
        .mask {
            position: absolute;
            top: 0;
            left: 0;
            z-index: 100;
            width: 100%;
            height: 100%;
            display: flex;

            // background-color: yellowgreen;
            .left {
                flex: 0 0 px2rem(150);

            }

            .right {
                flex: 0 0 px2rem(150);

            }

            .center {
                flex: 1;

            }
        }
    }

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
}
</style>
