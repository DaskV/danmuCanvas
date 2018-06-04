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
      src:'https://gz189cloud2.oos-gz.ctyunapi.cn/06694b99-061a-41dc-96f1-67ebc2bc1b7f?response-content-type=video/mp4&Expires=1528131786&response-content-disposition=attachment%3Bfilename%3D%22Sword%20Art%20Online%20Alternative%20GGO%2008%20%5B761869F1%5D.mp4%22&AWSAccessKeyId=6667aad7f6576995b9ae&Signature=lBbrzm37OmkSIyrHr2e0DWA1xLc%3D'
    }
  },
  mounted(){

  },
  components:{videoPlayer}
}
</script>

<style>

</style>
