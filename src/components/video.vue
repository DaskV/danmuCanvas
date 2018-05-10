<template>
    <div id="daskV-player" class="daskV-player">       
        <!-- 播放区域 -->
        <div class="daskV-player-section">
            <!-- 加载界面 -->
            <div class="daskV-player-preview">
                <div class="daskV-preview-icon"></div>
                <div class="daskV-preview-text">
                    <div class="daskV-preview-text-row">播放器初始化...['完成']</div>
                    <div class="daskV-preview-text-row">播放器初始化...['完成']</div>
                </div>
            </div>
            <!-- loading -->
            <div class="daskV-player-loding"></div>     
            <!-- 悬浮播放按钮 -->
            <div class="daskV-player-play"></div>
            <!-- 弹幕播放器 -->
            <barrage />
            <!-- 视频播放器 -->
            <div class="daskV-player-video">
                <video :src="sourceSrc"></video>
            </div>        
        </div>
        <!-- 控制台 -->
        <div class="daskV-player-controler">
            <!-- 暂停播放按钮 -->
            <div class="daskV-player-controler-pausePlay daskV-player-btn">
                <i class="iconfont">&#xe653;</i>               
            </div>
            <!-- 下一个按钮 -->
            <div class="daskV-player-controler-next daskV-player-btn">
                <i class="iconfont">&#xe624;</i>
            </div>
            <!-- 进度条 -->
            <div class="daskV-player-controler-progressBar" ref="progressBar">
                <progressBar  :progressWidth="progressWidth"  v-if="progressWidth" />
            </div>
            <!-- 设置区域 -->
            <div class="daskV-player-controler-config">
                <!-- 计时器 -->
                <div class="daskV-player-controler-timer daskV-player-btn">
                    <div class="daskV-player-controler-time-now">00:01</div>
                    <div class="daskV-player-controler-time-divider">/</div>
                    <div class="daskV-player-controler-time-total">99:99</div>
                </div>
                <!-- 声音 -->
                <div class="daskV-player-controler-voice daskV-player-btn">
                    <i class="iconfont" v-show="!voice.noVoice">&#xe625;</i>
                    <i class="iconfont" v-show="voice.noVoice">&#xe620;</i>
                    <div class="daskV-player-controler-voice-handle" >
                        <span class="daskV-player-controler-voice-value">{{voice.value}}</span>
                        <slider  @change="voiceChange" :vertical="true" />
                    </div>
                </div>
                <!-- 画质切换 -->
                <div class="daskV-player-controler-qualitymenu daskV-player-btn">
                    <div>720P</div>
                    <ul class="daskV-player-controler-quliatymenu-selector">
                        <li class="daskV-player-controler-quliatymenu-row" :class="{'active':quality.activeIndex === index }" v-for="(item,index) in quality.data" :key="index" @click="setQuality(index)">{{item}}</li>
                    </ul>
                </div>
                <!-- 弹幕设置 -->
                <div class="daskV-player-controler-barrageSet daskV-player-btn">
                    <i class="iconfont">&#xe67f;</i>
                    <div class="daskV-player-controler-barrageSet-wrap">
                        <slider @change="barrageChange" :vertical="false" />
                    </div>
                </div>
            </div>
        </div>
        <!-- 弹幕输入区 -->
        <div class="daskV-player-barrage"></div>
    </div>
</template>

<script>
import barrage from './barrage'
import progressBar from './progress'
import slider from './slider'
export default {
  data() {
    return {
        sourceSrc: '',
        progressWidth:'',
        voice:{
            noVoice:false,
            value:0,
        },
        quality:{
            data:['1080P','720P','480P'],
            activeIndex:0,
        }
    };
  },
  created() {},
  mounted() {
     this.progressWidth = this.$refs.progressBar.offsetWidth
  },
  methods: {
      voiceChange(val){
          this.voice.value = val
          this.voice.noVoice = val === 0 ? true : false 
      },
      setQuality(i){
          this.quality.activeIndex = i
      },
      barrageChange(val){

      }
  },
  components:{barrage,progressBar,slider}
};
</script>

<style lang="scss" scoped>
@font-face {font-family: "iconfont";
    src: url('../assets/font/iconfont.eot'); /* IE9*/
    src: url('../assets/font/iconfont.eot#iefix') format('embedded-opentype'), /* IE6-IE8 */
    url('../assets/font/iconfont.woff') format('woff'), /* chrome, firefox */
    url('../assets/font/iconfont.ttf') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+*/
    url('../assets/font/iconfont.svg#iconfont') format('svg'); /* iOS 4.1- */
 
}
$fontSize:12px;
$fontColor:#99a2aa;
.iconfont {
    font-family:"iconfont" !important;
    font-size:16px;
    font-style:normal;
    -webkit-font-smoothing: antialiased;
    -webkit-text-stroke-width: 0.2px;
    -moz-osx-font-smoothing: grayscale;
    color: #99a2aa;
}

@mixin backgourndImageCenter($percent){
    background-repeat: no-repeat;
    background-position: center;
    background-size: $percent;
}
$border:1px solid #e2e2e2;
.daskV-player *{
    user-select: none;
}
.daskV-player {
    position: relative;
    pointer-events:auto;
    width: 100%;
    height: 100%; 
    font-family: Arial, Helvetica, sans-serif;
    .daskV-player-section{
        position: relative;
        z-index: 69;
        width: 100%;
        height: 100%;
        user-select: none;
        border: $border;
        .daskV-player-preview{
            width: 100%;
            height: 100%;
            position: absolute;
            left: 0;
            top: 0;
            z-index: 13;
            background: #fff;
            display: none;
            cursor: pointer;
            .daskV-preview-icon{
                position: absolute;
                left: 50%;
                top: 50%;
                width: 90px;
                height: 90px;
                margin-left: -45px;
                margin-top: -45px;
                background-image: url('../assets/images/tv.gif');
                @include backgourndImageCenter(80%)
            }
            .daskV-player-text{
                position: absolute;
                color: #ccd0d7;
                font-size: 12px;
                left: 10px;
                bottom: 10px;
                line-height: 20px; 
            }
        }
        .daskV-player-loding{
            position: absolute;
            left: 50%;
            top: 50%;
            width: 50px;
            height: 50px;
            margin-left: -25px;
            margin-top: -25px;
            background-image: url('../assets/images/loading.gif');
            @include backgourndImageCenter(70%);
            display: none;
        }
        .daskV-player-play{
            display: block;
            width: 100px;
            height: 84px;
            position: absolute;
            right: 10px;
            bottom: 24px;
            cursor: pointer;
            z-index: 12;
            background-image: url('../assets/images/play.png');
            @include backgourndImageCenter(75%)
        }
        .daskV-player-video{
            position: relative;
            width: 100%;
            height: 100%;
            z-index: 10;
            video{
                width: 100%;
                height: 100%;
                margin: 0 auto;
            }
        }
    }
    .daskV-player-controler{
        display: flex;
        flex-flow: row nowrap;
        align-items: center;
        width: 100%;
        height: 30px;
        border: $border;
        border-top: 0;
        font-size: $fontSize;
        color:$fontColor;
        .daskV-player-controler-pausePlay,.daskV-player-controler-next{
            width: 6%;
            height: 100%;
            display: flex;
            justify-content:space-around;
            align-items: center;
            cursor: pointer;           
        }

        .daskV-player-btn:hover{
            background-color: #f4f5f7;       
            transition: all 0.3s linear;
            cursor: pointer;
            .iconfont{
                color: #6d757a;
                transition: all 0.3s linear;
            }
        }
        .daskV-player-controler-progressBar{
            width:50%;
        }
        .daskV-player-controler-config{
            width: 37%;
            height: 100%;
            padding-left:1%; 
            display: flex;
            flex-flow:row nowrap;
            justify-content: flex-start;            
            align-items: center;
            .daskV-player-controler-timer{
                width: 25%;
                height: 100%;
                display: flex;
                justify-content: space-between;
                align-items: center;
            }
            .daskV-player-controler-voice{
                width: 10%;
                height: 100%;
                line-height: 30px;
                text-align: center;
                position: relative;
                &:hover{
                    .daskV-player-controler-voice-handle{
                        visibility: visible;
                    }
                }
                .daskV-player-controler-voice-handle{
                    visibility: hidden;
                    width: 100%;
                    height: 100px;
                    position: absolute;
                    top: -145px;
                    left: -.5px;
                    background: #fff;
                    border-radius: 4px 4px 0 0;
                    border:$border;
                    z-index: 99;
                    padding: 35px 0 10px 0;
                    .daskV-player-controler-voice-value{
                        position: absolute;
                        top: 0;
                        left: 0;
                        text-align: center;
                        width: 100%;
                        height: 35px;
                    }
                }
            }
            .daskV-player-controler-qualitymenu{
                width: 15%;
                height: 100%;
                line-height: 30px;
                text-align: center;
                position: relative;
                &:hover{
                    .daskV-player-controler-quliatymenu-selector{
                        display: block;
                    }
                }
                .daskV-player-controler-quliatymenu-selector{
                    display: none;
                    position: absolute;
                    top: -100px;
                    left:50%;
                    margin-left: -39px;
                    z-index: 99;
                    background: #fff;
                    padding: 0;
                    .daskV-player-controler-quliatymenu-row{
                        padding: 0 20px;
                        transition: all .3s linear;
                        &:hover{
                            background: #e5e9ef;
                            color: #333;
                        }
                    }
                    .active{
                        background: #fff !important;
                        color: #00a1d6 !important;
                    }
                }
            }
            .daskV-player-controler-barrageSet{
                width: 10%;
                height: 100%;
                line-height: 30px;
                text-align: center;
                position: relative;
                &:hover{
                    .daskV-player-controler-barrageSet-wrap{
                        display: block;
                    }
                }
                .daskV-player-controler-barrageSet-wrap{
                    display: none;
                    position: absolute;
                    width: 200px;
                    height: 150px;
                    left:50%;
                    margin-left: -100px;
                    top: -151px;
                    z-index: 99;
                    background: #fff;
                    border-radius: 4px 4px 0 0;
                }
            }
        }
        
    }
    

}
</style>

