<template>
<div id="entry" @click.stop="enter">
  <img :class="{
    yoko: bg,
    tate: !bg
  }" :src="coverSrc" />
  <img src="../../res/img/touchBase.png" class="touch" id="touchBase" />
  <img src="../../res/img/touch.png" id="touch" class="touch twinkle" :class="{ active: isTouched }" />
</div>
</template>

<script>
import fs from 'fs'
import getPath from '../../js/common/getPath.js'
export default {
  data () {
    return {
      bg: null,
      isTouched: false,
      coverSrc: './img/2ndAnniversary.jpg'
    }
  },
  methods: {
    enter () {
      if (!this.isTouched) {
        this.isTouched = true
        this.playSe(new Audio('./asset/sound/se/se_title_start.mp3'))
        this.$emit('touch')
        setTimeout(() => {
          this.$emit('enter')
        }, 1000)
      }
    }
  },
  beforeMount () {
    this.$nextTick(() => {
      let msrEvent = localStorage.getItem('msrEvent')
      if (msrEvent) {
        let o = JSON.parse(msrEvent)
        if (fs.existsSync(getPath(`./public/img/card/bg_${o.card}.png`))) {
          this.coverSrc = `./img/card/bg_${o.card}.png`
        }
      }
    })
  },
  mounted () {
    this.$nextTick(() => {
      this.bg = (window.innerWidth / window.innerHeight >= 1280 / 824)
      window.addEventListener('resize', () => {
        this.bg = (window.innerWidth / window.innerHeight >= 1280 / 824)
      }, false)
    })
  }
}
</script>

<style>
#entry{
  position: absolute;
  z-index: 1000;
}
.yoko{
  position: fixed;
  z-index: 1000;
  bottom: 0;
  width: 100%;
}
.tate{
  position: fixed;
  z-index: 1000;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  height: 100%;
}
.touch{
  z-index: 1000;
  position: fixed;
  left: 50%;
  top: 90%;
}
#touch{
  margin-top: -17px;
  margin-left: -150px;
}
#touchBase{
  margin-top: -25px;
  margin-left: -264px;
}
.twinkle{
  animation:twinkle 1.6s linear infinite;
}
.twinkle.active{
  animation:twinkle 0.2s linear infinite;
}
/* .out{
  animation:out 0.2s linear;
} */

@keyframes twinkle{
  0%   {opacity: 1;}
  50%  {opacity: 0.5;}
  100% {opacity: 1;}
}

/* @keyframes out{
  0%   {opacity: 1;}
  100% {opacity: 0;}
} */

</style>
