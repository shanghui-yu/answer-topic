<template>
  <canvas id="canvas" width="80" height="80"></canvas>
</template>

<script>
export default {
  props:['qsIndex','lock'],
  data () {
    return {
      countDownSeconds:15,
      currentSeconds:15
    }
  },
  created () {
    this.drawDoubleCircle()
    this.interval_id = setInterval(()=>{
      this.drawDoubleCircle()
    }, 1e3);
  },
  watch: {
    qsIndex (val, oldVal) {
      if (this.countDownSeconds>=11) {
        this.countDownSeconds = 15
        this.currentSeconds = 15
        this.drawDoubleCircle()
        this.interval_id = setInterval(()=>{
          this.drawDoubleCircle()
        }, 1e3);
      }
      if (val =='10') {
        clearInterval(this.interval_id);
      }
    },
    lock (val, oldVal) {
       if(val!='false'){
         clearInterval(this.interval_id);
         this.countDownSeconds = 15
         this.currentSeconds = 15
         this.drawDoubleCircle()
       }
    }
  },
  mounted () {
  },
  methods: {
    drawDoubleCircle() {
      if (this.currentSeconds <= 0) {
        clearInterval(this.interval_id);
        this.countDownSeconds = 15
        this.currentSeconds = 15
        this.$emit('stopCall')
      }
      let canvasElement = document.getElementById('canvas');
      if (!canvasElement){
        clearInterval(this.interval_id)
        return
      } 
      let context = canvasElement.getContext('2d');
      let progress = 360 * this.currentSeconds / this.countDownSeconds;
      let progress_pi = Math.PI * (progress / 180 - 1 / 2);
      context.beginPath();
      context.arc(40, 40, 35, 0, Math.PI * 2, false);
      context.lineWidth=5;
      context.strokeStyle='#f5ff5b';
      context.stroke();
      context.closePath();

      context.beginPath();
      context.arc(40, 40, 35, -Math.PI * 1 / 2, progress_pi, false);
      context.lineWidth=5;
      context.strokeStyle='#0f49c0';
      context.stroke();
      context.closePath();

      context.beginPath();
      context.arc(40, 40, 33, 0, Math.PI * 2, false);
      context.closePath();
      context.fillStyle = 'white';
      context.fill();

      context.font = "bold 30px Arial";
      context.fillStyle = "red";
      context.textAlign = "center";
      context.textBaseline = 'middle';
      context.fillText(this.currentSeconds, 40, 40);
      // 抗锯齿
      context.globalCompositeOperation = 'source-atop';
      this.currentSeconds--;
    }
  }
}
</script>

<style scoped lang="less">

</style>
