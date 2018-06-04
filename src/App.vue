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
      // src:'http://www.w3school.com.cn/example/html5/mov_bbb.mp4'
      src:'https://cloud189-nj.oos-js.ctyunapi.cn/68e09e6a-3a50-4f7b-ba59-968d61dcb771?response-content-type=video/mp4&Expires=1528109355&response-content-disposition=attachment%3Bfilename%3D%22%5BUHA-WINGS%5D%5BKakegurui%5D%5B01%5D%5B720p%5D%5BGB%5D.mp4%22&AWSAccessKeyId=ad2d051c334eb8bbf4c1&Signature=HIXIRKNLCJZeDNAeg/B4N%2B7EYwI%3D'
    }
  },
  mounted(){

  },
  components:{videoPlayer}
}
</script>

<style>

</style>
