<template>
  <div class="V-radio">
      <div class="V-radio-item" v-for="(item,index) in list" :key="item.value" @click="checkItem(item.value,index)">
          <span class="V-radio-item-name" :class="{'active':index===activeIndex}" >{{item.text}}</span>
      </div>
      
  </div>
</template>

<script>
export default {
    data(){
        return{
            activeIndex:''
        }
    },
    props:{
        list:{
            type:Array,
            default:[]
        }
    },
    methods:{
        checkItem(val,index){
            this.activeIndex = index
            this.$emit('setFontSize',val)
        }
    }
}
</script>

<style lang="scss" scoped>
    .V-radio{
        .V-radio-item{        
            display: inline-block;
            margin-right: 10px;
            color: #222;
            cursor: pointer;
            position: relative;
            font-size: 12px;
            .V-radio-item-name{
                border-radius: 4px;
                border:1px solid transparent;
                padding: 4px 5px;
            }
            &:last-child{
                margin-right:0;
            }     
            .active{
                border: 1px solid #00a1d6;
                color: #00a1d6;
            }     
        }
        
    }
</style>

