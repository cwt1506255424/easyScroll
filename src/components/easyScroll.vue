<template>
  <div ref="easyContent" style="overflow-y: hidden">
    <div class="easyPoint easyPointDown" v-if="needPullDown">下拉圆点</div>
    <slot></slot>
    <div class="easyPoint easyPointUp" v-if="needPullUp" ref="easyPointUp">上拉圆点</div>
  </div>
</template>

<script>
  export default {
    name: "easyScroll",
    props: {
      //是否需要下拉刷新
      needPullDown: {
        type: Boolean,
        default: true
      },
      //是否需要上拉加载更多
      needPullUp: {
        type: Boolean,
        default: true
      },
      //需要监听父组件传递的数据，因为如果父组件是动态获取的数据，那么滚动需要在父组件变化的时候而变化
      easyData: {
        type: [Object, Array],
        required: true
      },
      //下拉刷新和上拉加载默认50px触发事件，可以修改
      easyPointHeight: {
        type: Number,
        default: 50
      }
    },
    methods: {
      //刷新dom高度的方法
      refreshHeight: function () {
        let that = this
        //firstElementChild

        //上拉的提示定位不准，需要修复
        this.$refs.easyPointUp.style.top = -2 * this.easyPointHeight + 'px'

        //拿到slot插槽中的dom
        let scrollDom = this.$refs.easyContent.children[1]

        //拿到slot插槽中的dom的所有的子元素
        let scrollChild = scrollDom.children

        /*设置top先隐藏加载圆点*/
        scrollDom.style.top = -this.easyPointHeight + 'px'

        //获取父组件中用户设置的需要滚动的高度
        let contentHeight = scrollDom.offsetHeight

        //将该组件的外层设置高度和slot插槽一样高
        this.$refs.easyContent.style.height = contentHeight + 'px'

        //根据scrollChild获取滚动元素的总高度
        let scrollChildLength = 0
        setTimeout(function () {
          for (let i = 0; i < scrollChild.length; i++) {
            scrollChildLength += scrollChild[i].offsetHeight
          }
        }, 20)

        //添加事件
        if (scrollDom) {
          scrollDom.addEventListener('touchstart', function (e) {
            that.initTouch = e.targetTouches[0].screenY
          })
          scrollDom.addEventListener('touchend', function (e) {
            scrollDom.style.top = -that.easyPointHeight + 'px'
            scrollDom.style.height = contentHeight + 'px'
          })
          scrollDom.addEventListener('touchmove', function (e) {
            //获取滚动的头部
            let scTop = scrollDom.scrollTop

            if (scTop === 0 && e.targetTouches[0].screenY - that.initTouch > 0) {
              console.log('要下拉了')
              that.setTop(e, scrollDom, that.initTouch)
            } else if (scrollChildLength === scTop + contentHeight && e.targetTouches[0].screenY - that.initTouch < 0) {
              console.log("要上拉了")
              that.setTop(e, scrollDom, that.initTouch)
            }
          })
        }
      },
      //设置top的方法
      setTop: function (e, scrollDom, initTouch) {
        let diffY = (e.targetTouches[0].screenY - initTouch) / 4
        if (Math.abs(diffY) <= this.easyPointHeight) {
          scrollDom.style.top = (diffY - this.easyPointHeight) + 'px'
        }
      }
    },
    watch: {
      easyData() {
        this.refreshHeight()
      }
    },
    data() {
      return {
        initTouch: 0
      }
    },
    mounted: function () {
      this.refreshHeight()
    }
  }
</script>

<style scoped>
  .easyContentBox {
    position: relative;
  }

  .easyPoint {
    height: 50px;
    vertical-align: middle;
    text-align: center;
    line-height: 50px;
    position: relative;
  }

  .easyPointDown {
    top: 0px;
  }

  .easyPointUp {
    bottom: 0px;
  }
</style>
