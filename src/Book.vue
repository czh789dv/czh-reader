<template>
<div class="book">
    <TitleBar :ifTitleAndMenuShow="ifTitleAndMenuShow"></TitleBar>

    <div class="read-wrapper">
        <div id="read"></div>
        <div class="mask">
            <div class="left" @click="prevPage()"></div>
            <div class="center" @click="toggleTitleAndMenu"></div>
            <div class="right" @click="nextPage()"></div>
        </div>
    </div>
    <MenuBar :ifTitleAndMenuShow="ifTitleAndMenuShow"></MenuBar>
</div>
</template>

<script>
import TitleBar from '@/components/TitleBar'
import MenuBar from '@/components/MenuBar'

//导入epubjs库
import Epub from 'epubjs'
//传递一个书的路径 后期会放在服务器
const DOWNLOAD_URL = '/static/三体.epub'

// global.ePub = Epub
//全局化

export default {
    components: {
        TitleBar,
        MenuBar
    },
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

}
</style>
