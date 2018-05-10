<template>
  <div class="daskV-slider">
      <div class="daskV-slider-wrap">
            <div class="daskV-slider-track" :class="{'vertical':vertical == true , 'horizontal':vertical === false}">

                <div class="daskV-slider-bar" ref="sliderBar" @click="barClick">
                    <div class="daskV-slider-bar-range" :style="{height:value+'px'}"></div>
                </div>
                <div class="daskV-slider-pointer" @mousedown="pointerDown" :style="{bottom:value + 'px'}" v-if="vertical"></div>
                <div class="daskV-slider-pointer" @mousedown="pointerDown" :style="{left:value + 'px'}" v-else></div>
                <div class="daskV-slider-textValue" v-if="!vertical" :style="{left:value + 'px'}">{{textValue}}</div>
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
            textValue:0,
        }
    },
    props:{
        pointBottom:{
            type:Number,
            default:0,
        },
        pointLeft:{
            type:Number,
            default:0,
        },
        vertical:{
            type:Boolean,
            default:false
        }
    },
    methods:{
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
                if(this.vertical){
                    if((e.clientY < this.barStyleBottom || e.clientY == this.barStyleBottom) && (e.clientY > this.barStyleTop || e.clientY == this.barStyleTop)){
                        this.value =  this.barStyleBottom - e.clientY
                    }
                }
                else{
                    if((e.clientX > this.barStyleLeft || e.clientX == this.barStyleLeft) && (e.clientX < this.barStyleRight || e.clientX == this.barStyleRight)){
                        let value  = e.clientX - this.barStyleLeft
                        this.value = value
                        this.textValue = Math.floor(value/2)     
                    }
                }
                this.$emit('change',this.value)          
            }             
        },
        //点击进度条
        barClick(e){
            this.value = this.vertical ? this.barStyleBottom - e.clientY : e.clientX - this.barStyleLeft
            if(!this.vertical) this.textValue = Math.floor((e.clientX - this.barStyleLeft)/2) 
            
            this.$emit('change',this.value)
        }
    },
    mounted(){
        if(this.vertical){
            this.barStyleTop = this.$refs.sliderBar.getBoundingClientRect().top
            this.barStyleBottom = this.$refs.sliderBar.getBoundingClientRect().bottom
        }
        else{
            this.barStyleLeft = this.$refs.sliderBar.getBoundingClientRect().left
            this.barStyleRight = this.$refs.sliderBar.getBoundingClientRect().right
        }
        
    },
}
</script>

<style lang="scss" scoped>
    .daskV-slider{
        width: 100%;
        height: 100%;
        .daskV-slider-wrap{
            position: relative;
            width: 100%;
            height: 100%;
            cursor: pointer;
            .vertical.daskV-slider-track{
                position: absolute;
                height: 100%;
                width: 6px;
                bottom: 0;
                border-radius: 4px;
                left: 50%;
                margin-left: -3px;
                background-color: #e5e9ef;
                .daskV-slider-bar{
                    height: 100%;
                    width: 6px;
                    position: absolute;
                    border-radius: 4px;
                    overflow: hidden;
                    bottom: 0;
                    left: 0;
                    .daskV-slider-bar-range{
                        position: absolute;
                        bottom: 0;
                        width: 100%;
                        left: 0;
                        background-color: #8adced;
                    }
                }
                .daskV-slider-pointer{
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
            .horizontal.daskV-slider-track{
                position: absolute;
                width: 100%;
                height: 6px;
                left: 0;
                border-radius: 4px;
                top: 50%;
                margin-top: -3px;
                background-color: #e5e9ef;
                .daskV-slider-bar{
                    width: 100%;
                    height: 6px;
                    position: absolute;
                    border-radius: 4px;
                    overflow: hidden;
                    top: 0;
                    left: 0;
                    .daskV-slider-bar-range{
                        height: 100%;
                        position: absolute;
                        top: 0;
                        left: 0;
                        background-color: #8adced;
                    }
                }
                .daskV-slider-pointer{
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
                .daskV-slider-textValue{
                    position: absolute;
                    top: -30px;
                    left: 0;
                }
            }
        }  
    }
</style>

