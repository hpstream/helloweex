<template>
  <div class="wrapper">
    <text class="greeting">The</text>
     <wxc-button text="Open Popup"
                @wxcButtonClicked="buttonClicked">
    </wxc-button>
     <wxc-popup width="500"
               pos="left"
               :show="isShow"
               @wxcPopupOverlayClicked="overlayClicked">
    </wxc-popup>
  </div>
</template>

<script>
import { WxcButton, WxcPopup } from 'weex-ui'

export default {
  components: { WxcButton, WxcPopup },
  name: 'App',
  data () {
    return {
      isShow: false
    }
  },
  methods: {
    buttonClicked () {
      this.isShow = true
    },
    overlayClicked () {
      this.isShow = false
    }
  }
}
</script>

<style scoped>
.logo {
  width: 50;
  height: 50;
}
.title {
  font-size: 42;
  color: #ff5400;
}
.wrapper {
  justify-content: center;
  align-items: center;
}
.logo {
  width: 424px;
  height: 200px;
}
.greeting {
  text-align: center;
  margin-top: 70px;
  font-size: 50px;
  color: #41b883;
}
.message {
  margin: 30px;
  font-size: 32px;
  color: #727272;
}
</style>
