<template>
  <canvas class="daskV-barrage-player" ref="player" style="position:absolute;top:0;left:0;width:100%;height:100%;z-index:13;background:#000;">
    您的浏览器不支持canvas标签。
  </canvas>
</template>

<script>
export default {
  data() {
    return {
      player: null,
      rect: null,
      width: null,
      height: null,
      context: null,
      font: "20px Microsoft YaHei",
      barrageList: []
    };
  },
  mounted() {
    this.player = this.$refs.player;
    this.rect = this.player.getBoundingClientRect();
    this.width = this.rect.right - this.rect.left;
    this.height = this.rect.bottom - this.rect.top;
    this.context = this.player.getContext("2d");
    this.context.font = this.font
    this.draw()
  },
  methods: {
    //绘制
    draw() {
      if (this.barrageList.length) {
        this.context.clearRect(0, 0, this.width, this.height);
        for (let i = 0; i < this.barrageList.length; i++) {
          let b = this.barrageList[i];
          if (b.left + b.width <= 0) {
            this.barrageList.splice(i, 1);
            i--;
            continue;
          }
          b.left -= b.offset;
          this.drawText(b);
        }
      }
      requestAnimationFrame(this.draw.bind(this));
    },
    //绘制文字
    drawText(barrage) {
      this.context.fillStyle = barrage.color;
      this.context.fillText(barrage.value, barrage.left, barrage.top);
    },
    //获取随机颜色
    getColor() {
      return "#" + Math.floor(Math.random() * 0xffffff).toString(16);
    },
    //获取随机top
    getTop() {
      //canvas绘制文字x,y坐标是按文字左下角计算，预留30px
      return Math.floor(Math.random() * (this.height - 30)) + 30;
    },
    //获取偏移量
    getOffset() {
      return +(Math.random() * 4).toFixed(1) + 1;
    },
    //发射弹幕
    shoot(value) {
      let top = this.top ||  this.getTop();
      let color = this.color || this.getColor();
      let offset = this.offset || this.getOffset();
      let width = Math.ceil(this.context.measureText(value).width);
      let barrage = {
        value: value,
        top: top,
        left: this.width,
        color: color,
        offset: offset,
        width: width
      };
      this.barrageList.push(barrage);
    }
  },
  props: {
    barrageOptions:{
      type:Object,
      default:()=>{
        return{
          top:null,
          color:null,
          offset:null
        }
      }
    }
  }
};
</script>

<style scoped>

</style>


