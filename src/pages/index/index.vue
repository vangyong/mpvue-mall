<template>
  <!--<div class="container">-->
    <!--<img class="girl" :src="imgSrc +'static/img/girl.png'" alt="">-->
    <!--<img class="logo" src="../../assets/logo.png" alt="">-->
    <!--<card :text="motto"></card>-->
    <!--<form class="form-container">-->
      <!--<input type="text" class="form-control" v-model="motto" placeholder="v-model" />-->
      <!--<input type="text" class="form-control" v-model.lazy="motto" placeholder="v-model.lazy" />-->
    <!--</form>-->
    <!--<a @click="gotoGame('pages/counter/main')" class="counter">去往Vuex示例页面</a>-->
    <!--<a @click="gotoGame('pages/logs/main')" class="counter">去往logs页面</a>-->
  <!--</div>-->

  <view class="container">
    <swiper class="banner" indicator-dots="true" autoplay="true" interval="3000" duration="1000">
      <swiper-item v-for="item of banner" :key="item.id">
        <navigator :url="item.link">
          <img :src="item.image_url" background-size="cover" />
        </navigator>
      </swiper-item>
    </swiper>
    <view class="m-menu">
      <navigator  class="item" :url="item.url" v-for="item of channel" :key="item.id">
        <img :src="item.icon_url" background-size="cover" />
        <text>{{item.name}}</text>
      </navigator>
    </view>
    <view class="a-section a-brand">
      <view class="h">
        <navigator url="../brand/brand">
          <text class="txt">品牌制造商直供</text>
        </navigator>
      </view>
      <view class="b">
        <view class="item item-1" v-for="item of brands" :key="item.id">
          <navigator :url="'/pages/brand/brandDetail?id='+item.id">
            <view class="wrap">
              <img class="img" :src="item.new_pic_url" mode="aspectFill" />
              <view class="mt">
                <text class="brand">{{item.name}}</text>
                <text class="price">{{item.floor_price}}</text>
                <text class="unit">元起</text>
              </view>
            </view>
          </navigator>
        </view>
      </view>
    </view>
    <view class="a-section a-new" v-if="newGoods.length">
      <view class="h">
        <view>
          <navigator url="../newGoods/newGoods">
            <text class="txt">周一周四 · 新品首发</text>
          </navigator>
        </view>
      </view>
      <view class="b">
        <view class="item" v-for="item of newGoods" :key="item.id">
          <navigator :url="'../goods/goods?id='+ item.id">
            <img class="img" :src="item.list_pic_url" background-size="cover" />
            <text class="name">{{item.name}}</text>
            <text class="price">￥{{item.retail_price}}</text>
          </navigator>
        </view>
      </view>
    </view>
    <view class="a-section a-popular" v-if="hotGoods.length">
      <view class="h">
        <view>
          <navigator url="../hotGoods/hotGoods">
            <text class="txt">人气推荐</text>
          </navigator>
        </view>
      </view>
      <view class="b">
        <view class="item" v-for="item of hotGoods" :key="item.id">
          <navigator :url="'/pages/goods/goods?id=' + item.id">
            <img class="img" :src="item.list_pic_url" background-size="cover" />
            <view class="right">
              <view class="text">
                <text class="name">{{item.name}}</text>
                <text class="desc">{{item.goods_brief}}</text>
                <text class="price">￥{{item.retail_price}}</text>
              </view>
            </view>
          </navigator>
        </view>
      </view>
    </view>
    <view class="a-section a-topic" v-if="topics.length">
      <view class="h">
        <view>
          <navigator url="../topic/topic" open-type="switchTab">
            <text class="txt">专题精选</text>
          </navigator>
        </view>
      </view>
      <view class="b">
        <scroll-view scroll-x="true" class="list">
          <view class="item" v-for="item of topics" :key="item.id">
            <navigator :url="'../topic/topicDetail?id=' + item.id">
              <img class="img" :src="item.scene_pic_url" background-size="cover" />
              <view class="np">
                <text class="name">{{item.title}}</text>
                <text class="price">￥{{item.price_info}}元起</text>
              </view>
              <text class="desc">{{item.subtitle}}</text>
            </navigator>
          </view>
        </scroll-view>
      </view>
    </view>
    <view class="good-grid" v-for="(item,index) of floorGoods" :key="item.id">
      <view class="h">
        <view>
          <text>{{item.name}}</text>
        </view>
      </view>
      <view class="b">
        <block v-for="(iitem,iindex) of item.goodsList" :key="iitem.id" :data-id="index">
          <view :class="  iindex % 2 == 0 ? 'item' : 'item item-b'">
            <navigator :url="'../goods/goods?id=' + iitem.id" class="a">
              <img class="img" :src="iitem.list_pic_url" background-size="cover" />
              <text class="name">{{iitem.name}}</text>
              <text class="price">￥{{iitem.retail_price}}</text>
            </navigator>
          </view>
        </block>
        <view class="item item-b item-more">
          <navigator :url="'/pages/category/category?id=' + item.id" class="more-a">
            <view class="txt">{{'更多'+item.name+'好物'}}</view>
            <image class="icon" src="../../static/images/icon_go_more.png" background-size="cover"/>
          </navigator>
        </view>
      </view>
    </view>
  </view>

</template>

<script>
// 组件引用
import card from '@/components/card'
import { mapState, mapActions } from 'vuex'


export default {
  computed: {
    ...mapState([
      'newGoods',
      'hotGoods',
      'topics',
      'brands',
      'floorGoods',
      'banner',
      'channel'
    ])
  },
  data () {
    return {
      motto: 'Hello World'
    }
  },
  components: {
    card
  },
  methods: {
     gotoGame (path) {
        this.reLaunchPageTo(this.router + path)
     }
  }
}
</script>

<style lang="less" scoped>
.userinfo {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.userinfo-avatar {
  width: 128/7.5vw;
  height: 128/7.5vw;
  margin: 20/7.5vw;
  border-radius: 50%;
}

.userinfo-nickname {
  color: #aaa;
}

.usermotto {
  margin-top: 150px;
}

.form-control {
  display: block;
  padding: 0 12px;
  margin-bottom: 5px;
  border: 1px solid #ccc;
  height:25px;
    text-overflow:clip;
    overflow:hidden;
    white-space:nowrap;
    -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
  -webkit-user-select: none;
  -moz-user-focus: none;
  -moz-user-select: none;
  -webkit-appearance:none;
  outline: none;
}

.counter {
  display: inline-block;
  margin: 10px auto;
  padding: 5px 10px;
  color: blue;
  border: 1px solid blue;
}
.container .girl{
    width: 275px;
    height: 400px;
}
.container .logo{
    width: 200px;
    height: 200px;
}

</style>
