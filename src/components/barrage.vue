<template>
  <canvas class="daskV-barrage-player" ref="player" :style="{'opacity':opacity}" :width="width" :height="height">
    您的浏览器不支持canvas标签。
  </canvas>
</template>

<style>
.daskV-barrage-player {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 13;
  background: transparent;
}
</style>


<script>
export default {
  data() {
    return {
      width: 0,
      height: 0,
      context: null,
      barrageList: []
    };
  },
  mounted() {
    let player = this.$refs.player;
    let rect = player.getBoundingClientRect();
    this.width = rect.right - rect.left;
    this.height = rect.bottom - rect.top;
    this.context = player.getContext("2d");
    this.draw();
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
      this.context.font = barrage.font;
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
    shoot(value, options) {
      let top = this.getTop();
      let color = options.color || this.getColor();
      
      let offset = this.getOffset();
      let font = `${options.fontSize} ${options.fontFamily}`;
      let width = Math.ceil(this.context.measureText(value).width);
      let barrage = {
        value: value,
        top: top,
        left: this.width,
        color: color,
        offset: offset,
        width: width,
        font: font,
      };
      this.barrageList.push(barrage);
    }
  },
  props: {
    opacity: {
      default: 1,
      type: Number
    }
  }
};
</script>

<style scoped>
</style>


