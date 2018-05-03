<template>
  <div id="app" style="width:500px;height:300px;background:#000">
    <player :options="options"  ref="danmu" />
  </div>
</template>

<script>
import player from './player.vue'

export default {
  data(){
    return{
       options:{
         width:500,
         height:300
       }
    }
  },
  mounted(){
    const textList = ['弹幕', '666', '233333333', 
        'javascript', 'html', 'css', '前端框架', 'Vue', 'React',
        'Angular','测试弹幕效果',
        '666', '233333333', 
        'javascript', 'html', 'css', '前端框架', 'Vue', 'React',
        'Angular','测试弹幕效果',
        '666', '233333333', 
        'javascript', 'html', 'css', '前端框架', 'Vue', 'React',
        'Angular','测试弹幕效果',
        '666', '233333333', 
        'javascript', 'html', 'css', '前端框架', 'Vue', 'React',
        'Angular','测试弹幕效果'
    ];
    textList.forEach((t) => {
        this.$refs.danmu.shoot(t);
    })
    
  },
  components:{player}
}
</script>

<style>

</style>
