<template>
  <div id="app">
    <!--<h1>hello easyScroll</h1>-->
    dhashdkjasdas
    <easy-scroll>
      <div class="easyContentBox">
        <div>你是
          <div v-for="(item,index) in this.dataList" class="listItem" @click="show">
            {{item}}
            <div>呵呵</div>
          </div>
        </div>
      </div>
    </easy-scroll>
    <div>
      dfnsakjnfkanfa
    </div>
  </div>
</template>

<script>
  import easyScroll from './components/easyScroll'

  export default {
    mounted: function () {
      for (let i = 1; i < 31; i++) {
        this.dataList.push(i)
      }
    },
    data() {
      return {
        dataList: []
      }
    },
    methods: {
      show: function () {
        alert("hahaha")
      }
    },
    components: {easyScroll},
    name: 'App'
  }
</script>

<style scoped>
  h1 {
    text-align: center;
    margin-top: 0px;
  }

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }

  .listItem {
    height: 50px;
  }

  .easyContentBox {
    background-color: aqua;
    height: 500px;
    overflow-y: scroll;
  }
</style>
