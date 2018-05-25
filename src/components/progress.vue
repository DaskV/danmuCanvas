<template>
  <div class="daskV-progress" :style="{width:progressWidth }"   >
        <div class="daskV-progress-warp">
            <div class="daskV-progress-track">
                <div class="daskV-progress-bar" @mousemove="getDetails" @mouseover="showDetails('block')" @mouseleave="showDetails('none')" ref="progressBar" @click="barClick">
                    <div class="daskV-progress-bar-range" :style="{width:rangeWidth+'px'}"></div>
                </div>
                <div class="daskV-progress-pointer" @mousedown="pointerDown" :style="{left:pointLeft + 'px'}" ></div>
            </div>
        </div>
        <div class="daskV-progress-details" :style="{display:detailsShow}">
            <div class="daskV-progress-details-time"></div>
            <div class="daskV-progress-details-sign" :style="{left:left}">
                <div class="daskV-progress-details-sign-down"></div>
                <div class="daskV-progress-details-sign-up"></div>
            </div>
        </div>
  </div>
</template>

<script>
export default {
    data(){
        return{
            left:0,
            detailsShow:false,
            pointerActive:false,
            pointLeft:0,
        }
    },
    methods:{
        getDetails(e){
            this.left = e.offsetX - 20 + 'px'                             
        },
        showDetails(type){                   
            this.detailsShow = type
        },
        //按下圆点
        pointerDown(){
            //mouseup mousemove 事件 必须在 down事件中回调，否则 mouseup事件将丢失
            this.pointerActive = true
            window.addEventListener('mousemove',this.pointerMove)
            window.addEventListener('mouseup',this.pointerUp)
        },
        //松开圆点
        pointerUp(){
            this.pointerActive = false
        },
        //移动圆点
        pointerMove(e){
            if(this.pointerActive){
                if((e.clientX > this.barStyleLeft || e.clientX == this.barStyleLeft) && (e.clientX < this.barStyleRight || e.clientX == this.barStyleRight)){
                    this.pointLeft = e.clientX - this.barStyleLeft
                }
            }             
        },
        //点击进度条
        barClick(e){
            this.pointLeft = e.clientX - this.barStyleLeft
        }
    },
    mounted(){
        this.barStyleLeft = this.$refs.progressBar.getBoundingClientRect().left
        this.barStyleRight = this.$refs.progressBar.getBoundingClientRect().right
    },
    props:['progressWidth','rangeWidth']
}
</script>

<style lang="scss" scoped>
    .daskV-progress{
        width: 100%;
        height: 28px;
        position: relative;
        .daskV-progress-warp{
            position: relative;
            width: 100%;
            height: 100%;
            cursor: pointer;
            .daskV-progress-track{
                position: absolute;
                width: 100%;
                height: 6px;
                left: 0;
                border-radius: 4px;
                top: 50%;
                margin-top: -3px;
                background-color: #e5e9ef;
                .daskV-progress-bar{
                    width: 100%;
                    height: 6px;
                    position: absolute;
                    border-radius: 4px;
                    overflow: hidden;
                    top: 0;
                    left: 0;
                    .daskV-progress-bar-range{
                        height: 100%;
                        position: absolute;
                        top: 0;
                        left: 0;
                        background-color: #8adced;
                    }
                }
                .daskV-progress-pointer{
                    position: absolute;
                    top: -4px;
                    height: 14px;
                    width: 14px;
                    border-radius: 7px;
                    cursor: pointer;
                    z-index: 15;
                    left: 0;
                    box-shadow: 0 0 3px #017cc3;
                    background-color: #fff;
                    transition: box-shadow .3s,-webkit-box-shadow .3s;
                }
            }
        }     
        .daskV-progress-details{
            position: absolute;
            top: -20px;
            left: 100px;
            overflow: visible;
            width: 160px;
            margin-left: -80px;
            text-align: center;
            z-index: 1;
            pointer-events: none;
            display: none;
            .daskV-progress-details-time{
                position: relative;
                z-index: 2;
                padding: 3px 5px;
                border-radius: 4px;
                line-height: 12px;
                height: 12px;
                font-size: 12px;
                background-color: #e5e9ef;
                color: #6b6b6b;
                vertical-align: top;
                display: inline-block;
            }
            .daskV-progress-details-sign{
                cursor: pointer;
                width: 8px;
                height: 16px;
                margin: 0 auto;
                position: absolute;
                overflow: hidden;
                top: 26px;
                left: 76px;
                .daskV-progress-details-sign-down{
                    width: 0;
                    height: 0;
                    border-width: 4px 4px 0;
                    border-style: solid;
                    border-color: #00a1d6 transparent transparent;
                    position: relative;
                }
                .daskV-progress-details-sign-up{
                        margin-top: 8px;
                        width: 0;
                        height: 0;
                        border-width: 0 4px 4px;
                        border-style: solid;
                        border-color: transparent transparent #00a1d6;
                        position: relative;
                }
            }
        } 
    }
</style>

