<template>
  <div id="app" style="width:800px;">
    <videoPlayer  :sourceSrc="src" />
  </div>
</template>

<script>
import videoPlayer from './components/video'

export default {
  data(){
    return{
      src:'http://www.w3school.com.cn/example/html5/mov_bbb.mp4'
    }
  },
  mounted(){

  },
  components:{videoPlayer}
}
</script>

<style>

</style>
