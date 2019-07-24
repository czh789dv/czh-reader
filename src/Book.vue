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
    <MenuBar
        :ifTitleAndMenuShow="ifTitleAndMenuShow"
        :fontSizeList="fontSizeList"
        :defaultfontSize="defaultfontSize"
        @setFontSize="setFontSize"
        :themeList="themeList"
        :defaultTheme="defaultTheme"
        @setTheme="setTheme"
        @onProgressChange="onProgressChange"
        :bookAvailable="bookAvailable"
        @jumpTo="jumpTo"
        :navigation="navigation"
        ref="menuBar"></MenuBar>
</div>
</template>

<script>
import TitleBar from "@/components/TitleBar";
import MenuBar from "@/components/MenuBar";

//导入epubjs库
import Epub from "epubjs";
import { truncate } from 'fs';
//传递一个书的路径 后期会放在服务器
const DOWNLOAD_URL = "/static/三体.epub";

// global.ePub = Epub
//全局化

export default {
    components: {
        TitleBar,
        MenuBar
    },
    data() {
        return {
            ifTitleAndMenuShow: false,
            fontSizeList: [{
                    fontSize: 12
                },
                {
                    fontSize: 14
                },
                {
                    fontSize: 16
                },
                {
                    fontSize: 18
                },
                {
                    fontSize: 20
                },
                {
                    fontSize: 22
                },
                {
                    fontSize: 24
                }
            ],
            defaultfontSize: 16,
            //主题设置
            themeList: [{
                name: 'default',
                style: {
                    body: {
                        'color': '#000',
                        'background': '#fff'
                    }
                }
            }, {
                name: 'eye',
                style: {
                    body: {
                        'color': '#000',
                        'background': '#ceeaba'
                    }
                }
            }, {
                name: 'night',
                style: {
                    body: {
                        'color': '#fff',
                        'background': '#000'
                    }
                }
            }, {
                name: 'gold',
                style: {
                    body: {
                        'color': '#000',
                        'background': 'rgb(241,236,226)'
                    }
                }
            }, ],
            //默认主题
            defaultTheme: 0,
            //图书默认可以用状态
            bookAvailable:false,
            navigation:{},
        };
    },
    methods: {
        //通过链接跳转到制定位置
        jumpTo(href){
            this.rendition.display(href)
            this.hideTitleAndMenu()
        },
        hideTitleAndMenu(){
            //隐藏菜单栏和设置栏
            this.ifTitleAndMenuShow=false
            this.$refs.menuBar.hideSetting()
            //隐藏目录
            this.$refs.menuBar.hideContent()
        },


        //progress进度条的数值 0-100变化
        onProgressChange(progress){
            //percentage为进度条百分比
            const percentage =progress /100
            //当前位置  百分比如果小于0 则为 0
            const location =percentage >0 ? this.locations.cfiFromPercentage(percentage) : 0
            //显示当前位置
            this.rendition.display(location)

        },
        //设置主题
        setTheme(index) {
            //选择传进来的index下标
            this.themes.select(this.themeList[index].name)
            //将主题设置为下标主题
            this.defaultTheme = index
        },
        registerTheme() {
            this.themeList.forEach(theme => {
                this.themes.register(theme.name, theme.style)
            })
        },
        setFontSize(fontSize) {
            //默认字号 等于当前字号
            this.defaultfontSize = fontSize;
            if (this.themes) {
                this.themes.fontSize(fontSize + "px");
            }
        },
        toggleTitleAndMenu() {
            this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow;
            if (!this.ifTitleAndMenuShow) {
                this.$refs.menuBar.hideSetting();
            }
        },
        //上一页
        prevPage() {
            //调用Rendition.prev方法
            if (this.rendition) {
                this.rendition.prev();
            }
        },
        //下一页
        nextPage() {
            //调用Rendition.next方法
            if (this.rendition) {
                this.rendition.next();
            }
        },
        //渲染电子书
        showEpub() {
            //生成BOOK实例对象
            this.book = new Epub(DOWNLOAD_URL);
            // console.log(this.book);
            //生成rendition
            this.rendition = this.book.renderTo("read", {
                //让书的宽高和屏幕一样
                width: window.innerWidth,
                height: window.innerHeight
            });
            //通过rendition.display渲染电子书
            this.rendition.display();
            //获取主题对象
            this.themes = this.rendition.themes;
            //设置默认字体
            this.setFontSize(this.defaultfontSize);
            //注册主题
            this.registerTheme()
            //默认样式
            this.setTheme(this.defaultTheme)
            //获取locations对象
            //直接调用不会生成 消耗太多资源
            this.book.ready.then(() => {
                //获取目录内容
                this.navigation=this.book.navigation
                //通过自带的钩子函数  回调返回locations对象
                return this.book.locations.generate()
            }).then(result => {
                //console.log(result)
                this.locations = this.book.locations
                //存储到本地变量
                this.bookAvailable=true
            })

        }
    },
    mounted() {
        //生命周期钩子函数
        this.showEpub();
    }
};
</script>

<style lang="scss" scoped>
@import "assets/styles/global.scss";

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
