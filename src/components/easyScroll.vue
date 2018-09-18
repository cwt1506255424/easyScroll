<template>
  <div ref="easyContent" style="overflow-y: hidden">
    <slot></slot>
  </div>
</template>

<script>
  export default {
    name: "easyScroll",
    data() {
      return {
        initTouch: 0
      }
    },
    mounted: function () {
      //console.log(this.$refs.easyContent.firstElementChild)
      let scrollDom = this.$refs.easyContent.firstElementChild;
      let scrollChild = scrollDom.children
      /*console.log(scrollChild)
      console.log(scrollChild.length)
      console.log(scrollChild[0])*/


      let easyContent = this.$refs.easyContent
      let contentHeight = easyContent.offsetHeight

      //获取滚动元素的总高度
      let scrollChildLength = 0
      setTimeout(function () {
        console.log(scrollChild.length)
        console.log(scrollChild[0])
        for (let i = 0; i < scrollChild.length; i++) {
          scrollChildLength += scrollChild[i].offsetHeight
        }
        console.log("滚动总高度" + scrollChildLength)
      }, 20)

      let bothHeight = 0
      if (scrollDom) {
        scrollDom.addEventListener('touchstart', function (e) {
          this.initTouch = e.targetTouches[0].screenY
        })
        scrollDom.addEventListener('touchend', function (e) {
          //console.log(e)
          scrollDom.style.top = 0
          easyContent.style.height = contentHeight + 'px'
        })
        scrollDom.addEventListener('touchmove', function (e) {
          let scTop = scrollDom.scrollTop
          //console.log(e)
          console.log(e.targetTouches[0].screenY)
          console.log("滚动条高度：" + scTop)
          console.log("滚动条高度：" + (scTop + contentHeight))
          if (scTop === 0 && e.targetTouches[0].screenY - this.initTouch > 0) {
            let diffY = (e.targetTouches[0].screenY - this.initTouch) / 4
            if (Math.abs(diffY) <= 50) {
              scrollDom.style.top = diffY + 'px'
              //easyContent.style.height = (contentHeight + Math.abs(diffY)) + 'px'
            }
            //scrollDom.transform = `translate3d(0,${e.targetTouches[0].screenY}px,0)`
          } else if (scrollChildLength === scTop + contentHeight && e.targetTouches[0].screenY - this.initTouch < 0) {
            console.log("要上拉了")
            let diffY = (e.targetTouches[0].screenY - this.initTouch) / 4
            if (Math.abs(diffY) <= 50) {
              scrollDom.style.top = diffY + 'px'
              //easyContent.style.height = (contentHeight + Math.abs(diffY)) + 'px'
            }
          }
        })
      }
    },
    methods: {}
  }
</script>

<style scoped>
  .easyContentBox {
    position: relative;
  }
</style>
