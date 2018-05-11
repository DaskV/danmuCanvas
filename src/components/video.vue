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
                    <i class="iconfont" v-show="!barrage.hide">&#xe698;</i>
                    <i class="iconfont" v-show="barrage.hide">&#xe697;</i>
                    <div class="daskV-player-controler-barrageSet-wrap">
                        <div class="daskV-player-controler-barrageSet-row">
                            <label>不透明度:</label>
                            <slider @change="barrageChange" :vertical="false" style="flex:1" :step="5" />
                        </div>
                        <div class="daskV-player-controler-barrageSet-row">
                            <label>隐藏弹幕:</label>
                            <input type="checkbox" v-model="barrage.hide" />
                        </div>
                    </div>
                </div>
                <!-- 全屏 -->
                <div class="daskV-player-controler-fullscreen daskV-player-btn">
                    <i class="iconfont">&#xe781;</i>
                </div>
            </div>
        </div>
        <!-- 弹幕输入区 -->
        <div class="daskV-player-sendbar">
            <!-- 设置弹幕字体 -->
            <div class="daskV-player-sendbar-fontset daskV-player-btn" @click="fontPick.show = !fontPick.show">
                <i class="iconfont">&#xe64f;</i>
            </div>
            <!-- 设置弹幕颜色 -->
            <colorPicker v-model="colorPick.value" ref="colorPicker" style="width:5%;">
                <div class="daskV-player-sendbar-fontcolor daskV-player-btn" @click="showPick">
                    <span class="corlorDisplay" v-show="colorPick.value" :style="{backgroundColor:colorPick.value}"></span>
                    <i class="iconfont" v-show="!colorPick.value">&#xe65b;</i>
                </div>
            </colorPicker>     
            <!-- 输入区域 -->
            <div class="daskV-player-texthandle">
                <input type="text" placeholder="您可以在这里输入弹幕哦~QAQ " class="daskV-player-texthandle-input" />
                <!-- 发送按钮 -->
                <div class="daskV-player-sendbtn">
                    <span>发 送》</span>
                </div>
            </div>        
            <!-- 字体选择器 -->       
            <transition name="slide-fade">
                <div class="daskV-player-fontpick-warp" v-show="fontPick.show">
                    <label>字号</label>
                    <radioer :list="fontPick.list"  />
                </div> 
            </transition>  
                      
        </div>
    </div>
</template>

<script>
import barrage from './barrage'
import progressBar from './progress'
import slider from './slider'
import colorPicker from './colorPicker'
import radioer from './radio'
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
        },
        barrage:{
            hide:false
        },
        colorPick:{
            value:''
        },
        fontPick:{
            show:false,
            value:'',
            list:[
                {
                    text:'小',
                    value:'16px'
                },
                {
                    text:'中',
                    value:'24px'
                },
                {
                    text:'大',
                    value:'32px'
                }
            ]
        }
    };
  },
  created() {},
  mounted() {
     this.progressWidth = this.$refs.progressBar.offsetWidth
     document.onclick = e => {
        if(!this.$el.contains(e.target)){
            this.fontPick.show = false   
            this.$refs.colorPicker.hideBox()
        }  
     }
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

      },
      showPick(){
          this.$refs.colorPicker.showBox()
      }
  },
  components:{barrage,progressBar,slider,colorPicker,radioer}
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

@mixin boxshadow(){
    box-shadow:0 0 5px rgba(0, 0, 0, 0.15);
}

@mixin borderRadius(){
    border-radius: 4px 4px 0 0;      
}

@mixin flexRow(){
    display: flex;
    flex-flow: row nowrap;
    align-items: center;
}

$border:1px solid #e2e2e2;

.slide-fade{
     transition: all .3s linear;
}
.slide-fade-leave-active,.slide-fade-enter-active {
  transition: all .3s linear;
}
.slide-fade-enter, .slide-fade-leave-to{
  transform: translateX(-10px);
  opacity: 0;
}

.daskV-player *{
    user-select: none;
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
.daskV-player {
    position: relative;
    pointer-events:auto;
    width: 100%;
    height: 100%; 
    font-family: Arial, Helvetica, sans-serif;
    border:$border;
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
        border-bottom: $border;
        border-top: 0;
        font-size: $fontSize;
        color:$fontColor;
        .daskV-player-controler-pausePlay,.daskV-player-controler-next{
            width: 5%;
            height: 100%;
            display: flex;
            justify-content:space-around;
            align-items: center;
            cursor: pointer;           
        }
        .daskV-player-controler-progressBar{
            flex:1
        }
        .daskV-player-controler-config{
            width: 30%;
            height: 100%;
            padding-left:1%; 
            display: flex;
            flex-flow:row nowrap;
            justify-content: flex-start;            
            align-items: center;
            .daskV-player-controler-timer{              
                height: 100%;
                display: flex;
                justify-content: space-between;
                align-items: center;
                flex-basis: 10%;
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
                    z-index: 99;
                    padding: 35px 0 10px 0;
                    border:$border;
                    @include borderRadius();
                    @include boxshadow();
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
                    border:$border;
                    @include borderRadius();
                    @include boxshadow();
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
                        visibility: visible;
                    }
                }
                .daskV-player-controler-barrageSet-wrap{
                    visibility: hidden;
                    position: absolute;
                    width: 170px;
                    left:50%;
                    margin-left: -100px;
                    top: -98px;
                    z-index: 99;
                    background: #fff;
                    border:$border;
                    @include borderRadius();
                    @include boxshadow();
                    padding: 20px 10px;
                    .daskV-player-controler-barrageSet-row{
                        @include flexRow();                        
                        label{
                            margin-right: 8px;
                        }
                    }
                }
            }
            .daskV-player-controler-fullscreen{
                line-height: 30px;
                text-align: center;
                height: 100%;
            }
        }
        .daskV-player-controler-config>div{
            flex:1
        }
    }
    .daskV-player-sendbar{
        height: 40px;
        line-height: 40px;
        @include flexRow();
        position: relative;
        .daskV-player-sendbar-fontset{
            width:5%;
            text-align: center;
            position: relative;
        }
        .daskV-player-sendbar-fontcolor{
            width:100%;
            text-align: center;
            position: relative;
            .corlorDisplay{
                width: 14px;
                height: 14px;
                display: inline-block;
                position: relative;
                top:3px;
                border-radius: 4px;
            }
        }
        .daskV-player-texthandle{
            flex: 1;
            background: linear-gradient(#f5f5f5,#fff);
            border-left: $border;
            @include flexRow();
            .daskV-player-texthandle-input{
                -webkit-appearance: none;
                height: 100%;
                border: none;
                flex:1;
                outline:none;
                padding-left: 10px;
                background: transparent;
                color:$fontColor;
            }
        }
        .daskV-player-sendbtn{
            color: #fff;
            border-radius: 4px;
            background-color: #00a1d6;
            vertical-align: middle;
            border: 1px solid #00a1d6;
            transition: .2s;
            outline: none;
            text-align: center;
            display: inline-block;
            overflow: visible;
            margin-right: 10px;
            z-index: 13;
            height: 28px;
            line-height: 28px;
            padding: 0 10px 0 14px;
            width: 40px;
            font-size: $fontSize;
            cursor: pointer;
            &:hover{
                background-color: #00b5e5;
                border-color: #00b5e5;
            }
        }
        .daskV-player-fontpick-warp{
            position: absolute;
            left:0;
            bottom: 40px;
            padding: 20px;
            background: #fff;
            z-index: 99;
            font-size: 12px;
            @include flexRow();
            color: $fontColor;
            @include borderRadius();
            border:$border;
            @include boxshadow();          
        }
    }
    .daskV-player-sendbar>div{
        height: 100%;
    }

}


</style>

