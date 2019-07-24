<template>
<div class="menu-bar">
    <transition name="slide-up">
        <div class="menu-wrapper" v-show="ifTitleAndMenuShow" :class="{'hide-box-shadow':ifSettingShow 
        || !ifTitleAndMenuShow }">
            <div class="icon-wrapper">
                <span class="icon-menu icon" @click="ShowSetting(3)"></span>
            </div>
            <div class="icon-wrapper">
                <span class="icon-progress icon" @click="ShowSetting(2)"></span>
            </div>
            <div class="icon-wrapper">
                <span class="icon-bright icon" @click="ShowSetting(1)"></span>
            </div>
            <div class="icon-wrapper">
                <span class="icon-a icon" @click="ShowSetting(0)">A</span>
            </div>
        </div>
    </transition>
    <transition name="slide-up">
        <div class="setting-wrapper" v-show="ifSettingShow">
            <div class="setting-font-size" v-if="showTag === 0">
                <div class="preview" :style="{fontSize:fontSizeList[0].fontSize + 'px'}">A</div>
                <div class="select">
                    <div class="select-wrapper" v-for="(item,index) in fontSizeList" :key="index" @click="setFontSize(item.fontSize)">
                        <div class="line"></div>
                        <div class="point-wrapper">
                            <div class="point" v-show="defaultfontSize === item.fontSize">
                                <div class="small-point"></div>
                            </div>
                        </div>
                        <div class="line"></div>
                    </div>
                </div>
                <div class="preview" :style="{fontSize:fontSizeList[fontSizeList.length - 1].fontSize + 'px'}">A</div>

            </div>
            <div class="setting-theme" v-else-if="showTag === 1">
                <div class="setting-theme-item" v-for="(item, index) in themeList" :key="index" @click="setTheme(index)">
                    <div class="preview" :style="{background: item.style.body.background}" :class="{'no-border':item.style.body.background !== '#fff'}"></div>
                    <div class="text" :class="{'selected': index === defaultTheme}">{{item.name}}</div>
                </div>
            </div>
            <div class="setting-progress" v-else-if="showTag === 2">
                <div class="progress-wrapper">
                    <!-- 进度条  -->
                    <input type="range" class="progress" max="100" min="0" step="1" @change="onProgressChange($event.target.value)" @input="onProgressInput($event.target.value)" :value="progress" :disabled="!bookAvailable" ref="progress">
                </div>
                <div class="text-wrapper">
                    <!-- 文本显示  可用的时候进度条 不可用显示加载 -->
                    <span>{{bookAvailable ? progress + '%' : '加载中.....'}}
                    </span>
                </div>
            </div>
        </div>
    </transition>
    <contentView :ifShowContent="ifShowContent" v-show="ifShowContent" :navigation="navigation" :bookAvailable="bookAvailable" @jumpTo="jumpTo"></contentView>
    <transition name="fade">
        <div class="content-mask" v-show="ifShowContent" @click="hideContent()"></div>

    </transition>
</div>
</template>

<script>
import ContentView from '@/components/ContentView'

export default {
    components: {
        ContentView,
    },
    props: {
        ifTitleAndMenuShow: {
            type: Boolean,
            default: false
        },
        fontSizeList: Array,
        defaultfontSize: Number,
        themeList: Array,
        defaultTheme: Number,
        bookAvailable: {
            type: Boolean,
            default: false
        },
        navigation: Object,

    },
    data() {
        return {
            ifSettingShow: false,
            showTag: 0,
            progress: 0,
            ifShowContent: false,
        }
    },
    methods: {
        // 跳转方法，调用父组件方法
        jumpTo(href) {
            this.$emit('jumpTo', href)
        },
        //隐藏目录
        hideContent() {
            this.ifShowContent = false
        },
        //使用父组件方法 传递已改变的进度条位置
        onProgressChange(progress) {
            this.$emit('onProgressChange', progress)
        },
        onProgressInput(progress) {
            this.progress = progress
            //根据进度改变颜色比利
            this.$refs.progress.style.backgroundSize = `${this.progress}% 100%`

        },
        setTheme(index) {
            this.$emit('setTheme', index)

        },
        //设置字号
        setFontSize(fontSize) {
            this.$emit('setFontSize', fontSize)
        },
        ShowSetting(tag) {
            this.showTag = tag
            if (this.showTag === 3) {
                this.ifSettingShow = false
                this.ifShowContent = true
            } else {
                this.ifSettingShow = true
            }
        },
        hideSetting() {
            this.ifSettingShow = false
        }
    },

}
</script>

<style lang="scss" scoped>
@import '../assets/styles/global.scss';

.menu-bar {
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

        &.hide-box-shadow {
            box-shadow: none
        }

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

    .setting-wrapper {
        z-index: 101;
        position: absolute;
        bottom: px2rem(48);
        left: 0;
        width: 100%;
        height: px2rem(60);
        background: white;
        box-shadow: 0px px2rem(-8) px2rem(8) rgba(0, 0, 0, 0.15);

        .setting-font-size {
            display: flex;
            height: 100%;

            .preview {
                flex: 0 0 px2rem(40);
                @include center;

            }

            .select {
                display: flex;
                flex: 1;

                .select-wrapper {
                    flex: 1;
                    display: flex;
                    align-items: center;

                    &:first-child {
                        .line {
                            &:first-child {
                                border-top: none;
                            }
                        }
                    }

                    &:last-child {
                        .line {
                            &:last-child {
                                border-top: none;
                            }
                        }

                    }

                    .line {
                        flex: 1;
                        height: 0;
                        border-top: px2rem(1) solid #ccc;

                    }

                    .point-wrapper {
                        position: relative;
                        flex: 0 0 0;
                        width: 0;
                        height: px2rem(7);
                        border-left: px2rem(1) solid #ccc;

                        .point {
                            position: absolute;
                            top: px2rem(-8);
                            left: px2rem(-10);
                            width: px2rem(20);
                            height: px2rem(20);
                            border-radius: 50%;
                            background: white;
                            border: px2rem(1) solid #ccc;
                            box-shadow: 0 px2rem(4) px2rem(4) rgba(0, 0, 0, 0.15);
                            @include center;

                            .small-point {
                                width: px2rem(5);
                                height: px2rem(5);
                                background: black;
                                border-radius: 50%;
                            }
                        }
                    }

                }
            }
        }

        .setting-theme {
            height: 100%;
            display: flex;

            .setting-theme-item {
                flex: 1;
                display: flex;
                flex-direction: column;
                padding: px2rem(5);
                box-sizing: border-box;

                .preview {
                    flex: 1;
                    border: px2rem(1) solid #ccc;
                    box-sizing: border-box;

                    &.no-border {
                        border: none;

                    }
                }

                .text {
                    flex: 0 0 px2rem(20);
                    font-size: px2rem(14);
                    color: #ccc;
                    @include center;

                    &.selected {
                        color: #333;
                        font-size: px2rem(16);

                    }
                }
            }
        }

        .setting-progress {
            position: relative;
            width: 100%;
            height: 100%;

            .progress-wrapper {
                width: 100%;
                height: 100%;
                @include center;
                padding: 0 px2rem(30);
                box-sizing: border-box;

                .progress {
                    width: 100%;
                    // 去除默认样式
                    -webkit-appearance: none;
                    height: px2rem(2);
                    // 背景颜色2种 左侧深灰 右侧浅灰
                    background: -webkit-linear-gradient(#999, #999) no-repeat, #ddd;

                    //绑定方法 根据进度改变颜色比利
                    background-size: 0 100%;

                    &:focus {
                        outline: none;
                    }

                    //进度条中间能动的那个小手柄
                    &::-webkit-slider-thumb {
                        -webkit-appearance: none;
                        height: px2rem(20);
                        width: px2rem(20);
                        border-radius: 50%;
                        background: white;
                        box-shadow: 0 4px 4px 0 rgba(0, 0, 0, 0.15);
                        border: px2rem(1) solid #dddddd;

                    }
                }
            }

            .text-wrapper {
                position: absolute;
                left: 0;
                bottom: 0;
                width: 100%;
                color: #333;
                font-size: px2rem(16);
                text-align: center;
            }
        }
    }

    .content-mask {
        position: absolute;
        top: 0;
        left: 0;
        z-index: 101;
        display: flex;
        width: 100%;
        height: 100%;
        background: rgba(51, 51, 51, 0.8)
    }

}
</style>
