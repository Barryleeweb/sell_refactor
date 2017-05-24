<template>
  <div>
    <v-header :seller="seller"></v-header> <!-- props传给header -->
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <keep-alive>  <!-- 如果把切换出去的组件保留在内存中，可以保留它的状态或避免重新渲染 -->
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
import {urlParse} from 'common/js/util'
import header from 'components/header/header.vue'
const ERR_OK = 0
export default {
  data () {
    return {
      seller: {
        id: (() => {
          let queryParam = urlParse()
          return queryParam.id
        })()
      }
    }
  },
  // vue-resource请求后端

  created () {
    this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
      response = response.body   // 不是json是body，看接口文档
      if (response.errno === ERR_OK) {
        this.seller = Object.assign({}, this.seller, response.data)
      }
    })
  },
  components: {
    'v-header': header
  }
}

</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl"

  .tab
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
     //border-bottom: 1px solid rgba(7, 17, 27, 0.1) //电脑看正常，dpi手机上看是2px
     border-1px(rgba(7, 17, 27, 0.1)) //手机1px,common/stylus/mixin.styl代码
    .tab-item
      flex: 1
      text-align: center
      & > a
        display: block
        font-size: 14px
        color: rgb(77, 85, 93)
        &.active
          color: rgb(240, 20, 20)
</style>
