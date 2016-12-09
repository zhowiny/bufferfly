<template>
  <div id="app">
    <div class="cnt">
      <div class="bufferfly" :style='{left:x+"px",top:y+"px",transitionDuration: (duration/1000)+"s, "+(duration/1000)+"s, .3s",transform:"rotate3d(0,0,1,"+(angle+90)+"deg)"}'>
        <span class="animated leftFly infinite left" /><span class="animated rightFly infinite right" /></div>
      <div class="content">
        <p>员工欢聚一堂<br>，共享欢乐时光<br>，增进团队凝聚力 <br>。</p>
        <p style="top:0.75rem;">总结上一年度工作任务<br>，放眼下一年度工作目标<br>；</p>
        <p style="top:-0.32rem;">年关将至<br>，感谢同仁们一年来的努力与付出<br>；</p>
        <h2 style="margin-left:0.64rem;top:-0.64rem;"/>
      </div>
      <div class="address">
        <p>时间 : 2017年1月8日 , 下午1:30</p>
        <p>地点 : 深圳市罗湖区桂园街道蔡屋围一街京基金融中心裙楼01层01-132b及02层01-207a号商铺3D coffee馆</p>
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    name: 'app',
    data () {
      return {
        x: Math.random() * document.body.scrollWidth - 50,
        y: Math.random() * document.body.scrollHeight - 50,
        duration: 500,
        timer: null,
        v: 0.08,
        angle: 0
      }
    },
    mounted () {
      this.init()
      this.initFontSize(document, window)
      for (let i = 0; i < 8; i++) {
        this.createFlower()
      }
    },
    methods: {
      initFlower () {
        let time = (Math.random() * 12 + 3)
        return {
          width: parseInt(Math.random() * 10 + 5) + 'px',
          height: parseInt(Math.random() * 10 + 5) + 'px',
          top: parseInt(Math.random() * 100) + 'px',
          left: parseInt(Math.random() * document.body.scrollWidth) + 'px',
          opacity: '1',
          transition: 'all ' + time.toFixed(3) + 's linear',
          time: time.toFixed(3) * 1000
        }
      },
      createFlower () {
        let flower = document.createElement('span')
        let flowerData = this.initFlower()
        flower.className = 'flowers'
        for (let i in flowerData) {
          flower.style[i] = flowerData[i]
        }
        document.querySelector('#app').appendChild(flower)
        setTimeout(() => {
          flower.style.top = parseInt(Math.random() * 100 + document.body.scrollHeight - 200) + 'px'
          flower.style.left = parseInt(Math.random() * document.body.scrollWidth) + 'px'
          flower.style.opacity = '0.2'
        }, 10)
        setTimeout(() => {
          document.querySelector('#app').removeChild(flower)
          this.createFlower()
        }, flowerData.time + 10)
      },
      initFontSize (e, t) {
        var maxWidth = 750 // 超过这个宽度不再改变font-size
        var defaultWidth = 750 // 当宽度为450px时,字体大小为10px
        var defaultSize = 100
        var d = e.documentElement
        var resize = 'orientationchange' in window ? 'orientationchange' : 'resize'
        var computedFontSize = function () {
          var clientWidth = d.clientWidth
          clientWidth > maxWidth && (clientWidth = maxWidth)
          d.style.fontSize = defaultSize * (clientWidth / defaultWidth) + 'px'
        }
        computedFontSize()
        e.addEventListener && t.addEventListener(resize, computedFontSize, !1)
      },
      init () {
        clearTimeout(this.timer)
        this.timer = setTimeout(() => {
          this.fly(parseInt(Math.random() * document.body.scrollWidth - 50), parseInt(Math.random() * document.body.scrollWidth - 50))
        }, this.duration)
      },
      fly (x, y) {
        let oldX = this.x
        let oldY = this.y
        let distance = Math.sqrt(Math.pow(oldX - x, 2) + Math.pow(oldY - y, 2)) // 距离
        let cos = (x - oldX) / distance // 余弦
        let radian = Math.acos(cos)  // 求出弧度
        let angle = 180 / (Math.PI / radian) // 用弧度算出角度
        if (y - oldY > 0) {
          this.angle = angle
        } else {
          this.angle = -angle
        }
        this.duration = distance / this.v // 过渡时间 = 距离/速度
        this.x = x
        this.y = y
        this.init()
      }
    }
  }
</script>
<style>
  @import "assets/fly.css";

  html, body, #app, .cnt {
    margin: 0;
    padding: 0;
    height: 100%;
    width: 100%;
    font-family: 'microsoft yahei';
  }

  .cnt {
    height: 13.34rem;
    background: url("assets/background.png") no-repeat center center;
    background-size: 7.5rem auto;

    position: relative;
    color: #555;
  }

  .content {
    margin: 0 auto;
    text-align: center;
    padding-top: 2.4rem;
  }

  .content p, .content h2 {
    display: inline-block;
    font-size: 0.32rem;
    width: 0.32rem;
    padding: 0.16rem;
    margin: 0;
    line-height: 0.32rem;
    vertical-align: bottom;
    word-break: break-all;
    position: relative;
  }

  .content h2 {
    background-image: url("./assets/title.png");
    background-repeat: no-repeat;
    background-size: 100% auto;
    padding: 0.16rem;
    height: 5.8rem;
    background-position: center center;
  }

  .bufferfly {
    width: 36px;
    height: 24px;
    background: url("./assets/body.png") no-repeat center center;
    background-size: auto 100%;
    position: fixed;
    z-index: 99;
    top: 0;
    left: 0;
    transition: top linear .5s, left linear .5s, transform linear .3s;
  }

  .bufferfly span {
    display: block;
    position: absolute;
    width: 16px;
    height: 24px;
    background-image: url("./assets/left.png");
    background-size: 100% 100%;
    left: 5.5%;
    top: 18%;
  }

  .bufferfly span.left {
    transform-origin: right center;
    -webkit-transform-origin: right center;
  }

  .bufferfly span.right {
    background-image: url("./assets/right.png");
    left: 50%;
    transform-origin: left center;
    -webkit-transform-origin: left center;
  }
  .address {
    width: 6.7rem;
    margin: 1.2rem auto;
  }
  .address p {
    font-size: 0.28rem;
    margin: 0;
  }
  .flowers {
    display: block;
    position: absolute;
    top:0;
    left:0;
    background: url("./assets/flower.png")no-repeat;
    background-size: 100% 100%;
  }
</style>
