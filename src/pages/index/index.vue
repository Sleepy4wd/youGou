<template>
  <div class="index-container">
    <div class="search-box">
      <input type="text" placeholder="搜索">
      <icon type="search" size="12"></icon>
    </div>
    <!-- 轮播图 -->
    <div class="swiper-container">
      <swiper indicator-dots autoplay circular indicator-active-color="#0094ff">
        <swiper-item v-for="item in swiperList" :key="item.image_src">
          <img mode="aspectFill" :src="item.image_src">
        </swiper-item>
      </swiper>
    </div>
    <!-- 分类按钮 -->
    <div class="category-container">
      <div class="item" v-for="item in categoryList">
        <img :src="item.image_src" alt>
        <p>{{item.name}}</p>
      </div>
    </div>
    <!-- 楼层区域 -->
    <div class="floor-container">
      <div class="floor" v-for="item in floorList">
        <!-- 顶部 -->
        <div class="top">
          <img :src="item.floor_title.image_src" alt>
          <h3>{{item.floor_title.name}}</h3>
        </div>
        <!-- 底部 -->
        <div class="bottom">
          <img v-for="(it,i)  in item.product_list" :src="it.image_src" alt>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// 导入hxios模块
import hxios from "../../utils/index.js";
export default {
  data() {
    return {
      // 轮播图数组
      swiperList: [],
      //分类按钮的数据
      categoryList: [],
      // 楼层数据
      floorList: []
    };
  },
  // 初始化的时候 获取数据
  async created() {
    let swiperRes = await hxios.get({
      url: "api/public/v1/home/swiperdata"
    });
    // console.log(swiperRes);
    this.swiperList = swiperRes.data.message;

    //分类按钮的数据
    let categoryRes = await hxios.get({
      url: "api/public/v1/home/catitems"
    });
    // console.log(categoryRes);
    this.categoryList = categoryRes.data.message;

    // 获取楼层数据
    let floorRes = await hxios.get({
      url: "api/public/v1/home/floordata"
    });
    console.log(floorRes);
    this.floorList = floorRes.data.message;
  }
};
</script>

<style lang="less">
@pRed: #ff2d4a;
.index-container {
  padding-top: 100rpx;
  .search-box {
    width: 100%;
    background-color: @pRed;
    padding: 20rpx 16rpx;
    position: fixed;
    left: 0;
    top: 0;
    box-sizing: border-box;
    input {
      width: 100%;
      height: 60rpx;
      padding-left: 376rpx;
      background-color: #fff;
      box-sizing: border-box;
      border-radius: 10rpx;
      font-size: 26rpx;
    }
    icon {
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
    }
  }
  .swiper-container {
    swiper {
      height: 340rpx;
      swiper-item {
        height: 100%;
      }
    }
    img {
      display: block;
      width: 100%;
      height: 100%;
    }
  }
  .category-container {
    display: flex;
    padding-top: 24rpx;
    padding: 29rpx;
    background-color: #fff;
    .item {
      flex: 1;
      img {
        display: flex;
        width: 128rpx;
        height: 128rpx;
        margin: 0 auto;
      }
      p {
        text-align: center;
        margin-top: 10rpx;
        font-size: 24rpx;
      }
    }
  }
  // 楼层数据
  .floor-container {
    .floor {
      .top {
        padding-top: 30rpx;
        padding-bottom: 30rpx;
        padding-left: 15rpx;
        position: relative;
        height: 90rpx;
        box-sizing: border-box;
        h3 {
          color: #ff7b94;
          position: absolute;
          left: 30rpx;
          top: 50%;
          transform: translateY(-50%);
        }
        img {
          position: absolute;
          height: 90rpx;
          left: 0;
          top: 0;
          display: block;
          width: 100%;
        }
      }
      .bottom {
        padding: 20rpx 0 0 16rpx;
        overflow: hidden;
        img {
          display: block;
          width: 33.333%;
          float: left;
          width: 230rpx;
          height: 190rpx;
          margin-right: 10rpx;
          &:first-child {
            width: 230rpx;
            height: 390rpx;
          }
          &:nth-child(2) {
            margin-bottom: 10rpx;
          }
          &:nth-child(3) {
            margin-bottom: 10rpx;
          }
        }
      }
    }
  }
}
</style>
