/* eslint-disable */
<template>
  <div class="box">
    <div class="dragbox" id="dragbox" ref="dragbox">
      <div class="dragcon">
        <div class="ball" id="ball" ref="ball"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DemoCSSJS',
  props: {
    msg: String
  },
  data() {
    return {
      isLiveDrop: true,
    }
  },
  mounted() {
    const dragbox = this.$refs.dragbox
    const ball = this.$refs.ball

    document.addEventListener('touchstart', (ev) => {
      if (!ball.contains(ev.target)) {
        dragbox.classList.add('move');
      }
    })
    document.addEventListener('touchend', () => {
      // window.is_start = false;
      dragbox.classList.remove('move');
    })
    dragbox.scrollLeft = 0;
    dragbox.scrollTop = dragbox.offsetHeight;

  },
  directives: {
    move: {
      inserted: (el) => {
        const setDomDrop = () => {
          // 鼠标按下的初始坐标
          let StartX = null
          let StartY = null
          // 鼠标移动时候的坐标
          let MoveX = null
          let MoveY = null
          // 鼠标的最终位置
          let EndX = null
          let EndY = null
          // 获取盒子的初始值
          let DOMLeft = null
          let DOMTop = null
          // 元素的宽高
          let DOMHeight = null
          let DOMWidth = null
          //当前元素移动的最大距离
          let MaxX = null
          let MaxY = null
          // 当前元素移动最小距离
          let MinX = 0
          let MinY = 0
          // 元素的最终位置
          let DOMEndLeft = null
          let DOMEndTop = null
          // const _this=this;
          // 鼠标按下
          el.onmousedown = function (e) {
            //鼠标按下事件
            e = e || window.event //事件对象
            StartX = e.clientX //鼠标按下X的坐标
            StartY = e.clientY //鼠标按下Y的坐标
            DOMLeft = el.offsetLeft //获取盒子的初始left值
            DOMTop = el.offsetTop //获取盒子的初始top值
            // 获取元素的宽
            DOMHeight = el.offsetHeight
            DOMWidth = el.offsetWidth
            console.log('this', el)
            console.log('this.offsetLeft', el.offsetLeft)
            console.log('this.offsetTop', el.offsetTop)
            console.log('this.offsetHeight', el.offsetHeight)
            console.log('this.offsetWidth', el.offsetWidth)
            // 获取元素的高
            document.onmousemove = function (e) {
              // 赋值为拖拽
              this.isLiveDrop = false;
              //鼠标移动事件
              e = e || window.event
              MoveX = e.clientX //鼠标移动X的坐标
              MoveY = e.clientY //鼠标移动Y的坐标
              //移动的坐标减去按下的坐标 = 移动的距离
              EndX = MoveX - StartX
              EndY = MoveY - StartY
              // 当前页面可视区域内宽高-元素宽高=当前元素移动的最大距离
              MaxX = document.documentElement.clientWidth - DOMHeight //页面高度
              MaxY = document.documentElement.clientHeight - DOMWidth //页面宽度
              // 元素最终位置
              DOMEndLeft = DOMLeft + EndX
              DOMEndTop = DOMTop + EndY
              // 判断元素位置是否到达最小位置
              if (DOMEndLeft <= MinX) {
                DOMEndLeft = MinX + DOMWidth / 2
              }
              if (DOMEndTop <= MinY) {
                DOMEndTop = MinY + DOMHeight / 2
              }
              // 判断元素是否达到最大位置
              if (DOMEndLeft >= MaxX) {
                DOMEndLeft = MaxX + DOMWidth / 2
              }
              if (DOMEndTop >= MaxY) {
                DOMEndTop = MaxY + DOMHeight / 2
              }
              //赋值给left和top
              el.style.top = DOMEndTop + 'px'
              el.style.left = DOMEndLeft + 'px'
            }
            //鼠标抬起事件
            document.onmouseup = function () {

              //清除移动和抬起事件
              this.onmousemove = this.onmouseup = null
              // 区分拖拽还是点击事件
              if (this.isLiveDrop) {
                // this.showLiveList()
              }
              this.isLiveDrop = true;
            }
            return false //阻止默认事件
          }
        }
        setDomDrop()
      }
    }
  },
  methods: {
  }

}






</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.box {
  width: 100vw;
  height: 100vh;
  background-image: url('../assets/wolf.jpg');
}
:-webkit-scrollbar {
  width: 0 !important;
}
::-webkit-scrollbar {
  width: 0 !important;
  height: 0;
}
html,
body {
  margin: 0;
}

section {
  padding: 10px;
}

.dragbox {
  position: fixed;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
  overflow: auto;
  -webkit-overflow-scrolling: touch;
}

.dragbox.move {
  overflow: hidden;
  pointer-events: none;
}

.dragcon {
  width: calc(200% - 100px);
  height: calc(200% - 100px);
}

.ball {
  position: relative;
  width: 100px;
  height: 100px;
  background-color: cornflowerblue;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  border-radius: 50%;
  pointer-events: all;
}
</style>
