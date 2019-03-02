<template>
  <wxc-tab-bar
    :tab-titles="tabTitles"
    :tab-styles="tabStyles"
    title-type="icon"
    @wxcTabBarCurrentTabSelected="wxcTabBarCurrentTabSelected"
  >
    <!-- 第一个页面内容-->
    <div
      class="item-container"
      :style="contentStyle"
    >
     <text v-for='it of items' :key='it.id'>{{it.id}}</text>
    </div>

    <!-- 第二个页面内容-->
    <div
      class="item-container"
      :style="contentStyle"
    >
      <text>特别推荐</text>
    </div>

    <!-- 第三个页面内容-->
    <div
      class="item-container"
      :style="contentStyle"
    ><text>消息中心</text></div>

    <!-- 第四个页面内容-->
    <div
      class="item-container"
      :style="contentStyle"
    ><text>我的主页</text></div>
  </wxc-tab-bar>
</template>

<style scoped>
.item-container {
  width: 750px;
  background-color: #f2f3f4;
}
</style>
<script>
import { WxcTabBar, wxcButton, Utils } from 'weex-ui'

// https://github.com/alibaba/weex-ui/blob/master/example/tab-bar/config.js
import Config from './utils/config.js'
const stream = weex.requireModule('stream')
const modal = weex.requireModule('modal') || {}
const API =
  'https://kitsu.io/api/edge/anime?filter%5Bstatus%5D=current&sort=-userCount&page%5Blimit%5D=20'
export default {
  components: { WxcTabBar, wxcButton },
  data: () => ({
    tabTitles: Config.tabTitles,
    tabStyles: Config.tabStyles,
    items: [],
    firstId: 1
  }),
  created () {
    const tabPageHeight = Utils.env.getPageHeight()
    // 如果页面没有导航栏，可以用下面这个计算高度的方法
    // const tabPageHeight = env.deviceHeight / env.deviceWidth * 750;
    const { tabStyles } = this
    this.contentStyle = { height: tabPageHeight - tabStyles.height + 'px' }
    const self = this
    stream.fetch(
      {
        method: 'GET',
        url: API,
        type: 'json'
      },
      function (ret) {
        if (!ret.ok) {
          modal.toast({
            message: 'Network Error!',
            duration: 3
          })
        } else {
          self.firstId = ret.data.data[0].id
          self.items = self.items.concat(ret.data.data)
        }
      }
    )
  },
  mounted () {
    this.ajax()
  },
  methods: {
    wxcTabBarCurrentTabSelected (e) {
      const index = e.page
      console.log(index)
    },
    ajax: function (e) {
      const self = this
      const offset = this.items.length

      stream.fetch(
        {
          method: 'GET',
          url: API + `&page%5Boffset%5D=${offset}`,
          type: 'json'
        },
        function (ret) {
          if (!ret.ok) {
            modal.toast({
              message: 'Network Error!',
              duration: 3
            })
          } else {
            self.items = self.items.concat(ret.data.data)
          }
        }
      )
    }
  }
}
</script>
