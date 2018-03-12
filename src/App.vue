<template>
  <div>
    <v-header v-bind:seller="seller"></v-header>
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
    <keep-alive>
      <router-view :seller="seller" keep-alive></router-view>
    </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
  import header from './components/header/header'
  import {urlParse} from "./common/js/util";

  const REE_OK = 0

  export default {
    data() {
      return {
        seller: {
          id: (() => {
            let queryParam = urlParse()
            return queryParam.id
          })()
        }
      }
    },
    created() {
      this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
        response = response.body
        if (response.errno === REE_OK) {
          this.seller = response.data
          // console.log(this.seller)
        }
      })
    },
    components: {
      'v-header': header
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "common/stylus/mixin.styl"
  .tab
    display: flex;
    width: 100%;
    line-height: 42px;
    height: 42px;
    border-1px(rgba(7, 17, 27, 0.1));

  .tab .tab-item
    flex: 1;
    text-align: center;

  .tab .tab-item a
    text-align: center;
    font-family: "微软雅黑";
    display: block;
    font-size: 14px;
    color: rgba(77, 85, 93, 1);

  .tab .tab-item .active
    color: rgba(240, 20, 20, 1);
</style>
