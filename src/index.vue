<template>
  <wxc-tab-bar :tab-titles="tabTitles"
               :tab-styles="tabStyles"
               title-type="icon"
               @wxcTabBarCurrentTabSelected="wxcTabBarCurrentTabSelected">
    <!-- 第一个页面内容-->
    <div class="item-container" :style="contentStyle">
      <text>首页</text>
      <div v-for='item of result' :key="item.hashId">
        {{item.content}}
      </div>
       <wxc-button text="确定"
              @wxcButtonClicked="wxcButtonClicked"></wxc-button>
    </div>

    <!-- 第二个页面内容-->
    <div class="item-container" :style="contentStyle"><text>特别推荐</text></div>

    <!-- 第三个页面内容-->
    <div class="item-container" :style="contentStyle"><text>消息中心</text></div>

    <!-- 第四个页面内容-->
    <div class="item-container" :style="contentStyle"><text>我的主页</text></div>
  </wxc-tab-bar>
</template>

<style scoped>
  .item-container {
    width: 750px;
    background-color: #f2f3f4;
    align-items: center;
    justify-content: center;
  }
</style>
<script>
import { WxcTabBar, wxcButton, Utils } from 'weex-ui'

// https://github.com/alibaba/weex-ui/blob/master/example/tab-bar/config.js
import Config from './utils/config.js'
const stream = weex.requireModule('stream')
export default {
  components: { WxcTabBar, wxcButton },
  data: () => ({
    tabTitles: Config.tabTitles,
    tabStyles: Config.tabStyles,
    result: []
  }),
  created () {
    const tabPageHeight = Utils.env.getPageHeight()
    // 如果页面没有导航栏，可以用下面这个计算高度的方法
    // const tabPageHeight = env.deviceHeight / env.deviceWidth * 750;
    const { tabStyles } = this
    this.contentStyle = { height: (tabPageHeight - tabStyles.height) + 'px' }
  },
  mounted () {
    this.wxcButtonClicked()
  },
  methods: {
    wxcButtonClicked () {
      console.log(1)
      stream.fetch({
        method: 'GET',
        type: 'jsonp',
        url: 'https://v.juhe.cn/joke/randJoke.php?key=b2bbac3e44840eb124aa325a55097fec'
      }, res => {
        if (res.ok) {
          console.log(res.data.result)
          this.result = res.data.result
        } else {
          this.count = '- unkonwn -'
        }
      })
    },
    wxcTabBarCurrentTabSelected (e) {
      const index = e.page
      console.log(index)
    }
  }
}
</script>
