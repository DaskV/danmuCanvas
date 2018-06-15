<template>
  <div class="V-slider">
      <div class="V-slider-wrap">
            <div class="V-slider-track" :class="{'vertical':vertical == true , 'horizontal':vertical === false}">

                <div class="V-slider-bar" ref="sliderBar" @click="barClick">
                    <div class="V-slider-bar-range" :style="pointStyle"></div>
                </div>
                <div class="V-slider-pointer" @mousedown="pointerDown" :style="rangeStyle"></div>
                <div class="V-slider-textValue" v-if="!vertical" :style="{left:value + 'px'}">{{value}}</div>
            </div>
      </div>
  </div>
</template>

<script>
export default {
    data(){
        return{
            pointerActive:false,
            value: this.vertical ? this.pointBottom : this.pointLeft,
            newPosition:null,
            rangeStyle:{},
            pointStyle:{}        
        }
    },
    props:{
        pointBottom:{
            type:Number,
            default:50,
        },
        pointLeft:{
            type:Number,
            default:0,
        },
        vertical:{
            type:Boolean,
            default:false
        },
        step:{
            type:Number,
            default:1,
        },
        max:{
            type:Number,
            default:100,
        },
        min:{
            type:Number,
            default:0
        }
    },
    computed:{
        currentPosition(){
            return `${ (this.value - this.min) / (this.max - this.min) * 100 }%`;
        }
    },
    methods:{
         //按下圆点
        pointerDown(e){
            //mouseup mousemove 事件 必须在 down事件中回调，否则 mouseup事件将丢失
            window.addEventListener('mousemove',this.pointerMove)
            window.addEventListener('mouseup',this.pointerUp)

            this.pointerActive = true
            //开始时的起始位置
            if(this.vertical){
                this.startY = e.clientY
            }
            else{
                this.startX = e.clientX
            }
            this.startPosition = parseFloat(this.currentPosition)
            this.newPosition = parseFloat(this.currentPosition) - this.step / (this.max - this.min) * 100
            this.setPosition(this.newPosition)
            
        },
        //松开圆点
        pointerUp(){
            this.pointerActive = false
        },
        //移动圆点
        pointerMove(e){
            if(this.pointerActive){                 
                let diff = 0
                if(this.vertical){
                    diff = (this.startY - e.clientY) / this.barHeight * 100
                }
                else{
                    diff = (e.clientX - this.startX) / this.barWidth * 100
                }
                this.newPosition = this.startPosition + diff
                this.setPosition(this.newPosition)
            }             
        },
        //点击进度条
        barClick(e){
            if(this.vertical){
                this.setPosition((this.barStyleBottom - e.clientY) / this.barHeight * 100)
            }
            else{
                this.setPosition((e.clientX - this.barStyleLeft) / this.barWidth * 100)
            }
        },
        //设置最新位置
        setPosition(newPosition){
            if(newPosition === null) return;
            if(newPosition < 0) newPosition = 0
            else if(newPosition > 100) newPosition = 100

            let lengthPerStep = 100 /  ((this.max - this.min) / this.step)  //进度条分成几份
            let steps = Math.round(newPosition / lengthPerStep)  //总步数
            let value = steps * lengthPerStep * (this.max - this.min) * 0.01 + this.min //当前实际值
            this.value = value.toFixed(0)
            this.rangeStyle = this.vertical ? { bottom : value +'px' } : { left : value +'px'}
            this.pointStyle = this.vertical ? { height : value +'px'} : { width: value +'px'}
            this.$emit("change",value)
        }
    },
    mounted(){
        if(this.vertical){
            this.barStyleTop = this.$refs.sliderBar.getBoundingClientRect().top
            this.barStyleBottom = this.$refs.sliderBar.getBoundingClientRect().bottom
            this.barHeight = this.$refs.sliderBar.offsetHeight
            this.value = this.pointBottom
            this.pointStyle = { height : this.value +'px'}
            this.rangeStyle = { bottom : this.value +'px'}          
        }
        else{
            this.barStyleLeft = this.$refs.sliderBar.getBoundingClientRect().left
            this.barStyleRight = this.$refs.sliderBar.getBoundingClientRect().right
            this.pointStyle = { width : this.value +'px'}
            this.rangeStyle = { left : this.value +'px'}
            this.barWidth = this.$refs.sliderBar.offsetWidth
        }
        this.$emit("change",this.value)
        
    },
}
</script>

<style lang="scss" scoped>
    .V-slider{
        width: 100%;
        height: 100%;
        .V-slider-wrap{
            position: relative;
            width: 100%;
            height: 100%;
            cursor: pointer;
            .vertical.V-slider-track{
                position: absolute;
                height: 100%;
                width: 6px;
                bottom: 0;
                border-radius: 4px;
                left: 50%;
                margin-left: -3px;
                background-color: #e5e9ef;
                .V-slider-bar{
                    height: 100%;
                    width: 6px;
                    position: absolute;
                    border-radius: 4px;
                    overflow: hidden;
                    bottom: 0;
                    left: 0;
                    .V-slider-bar-range{
                        position: absolute;
                        bottom: 0;
                        width: 100%;
                        left: 0;
                        background-color: #8adced;
                    }
                }
                .V-slider-pointer{
                    position: absolute;
                    bottom: 0;
                    left: -4px;
                    height: 14px;
                    width: 14px;
                    border-radius: 7px;
                    cursor: pointer;
                    z-index: 15;                 
                    box-shadow: 0 0 3px #017cc3;
                    background-color: #fff;
                    transition: box-shadow .3s,-webkit-box-shadow .3s;
                }
            }
            .horizontal.V-slider-track{
                position: absolute;
                width: 100%;
                height: 6px;
                left: 0;
                border-radius: 4px;
                top: 50%;
                margin-top: -3px;
                background-color: #e5e9ef;
                .V-slider-bar{
                    width: 100%;
                    height: 6px;
                    position: absolute;
                    border-radius: 4px;
                    overflow: hidden;
                    top: 0;
                    left: 0;
                    .V-slider-bar-range{
                        height: 100%;
                        position: absolute;
                        top: 0;
                        left: 0;
                        background-color: #8adced;
                    }
                }
                .V-slider-pointer{
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
                .V-slider-textValue{
                    position: absolute;
                    top: -30px;
                    left: 0;
                }
            }
        }  
    }
</style>

