<template>
  <div class="home">
    <!--search bar layout-->
    <div class="search-bar">
      <van-row>
        <van-col span="3">
          <img :src="locationIcon" width="70%" class="location-icon" />
        </van-col>
        <van-col span="16">
          <input type="text" class="search-input" />
        </van-col>
        <van-col span="5">
          <van-button size="mini">查找</van-button>
        </van-col>
      </van-row>
    </div>
    <!--swipwer area-->
    <div class="swiper-area">
      <van-swipe :autoplay="1000">
        <van-swipe-item v-for="(banner, index) in bannerPicArray" :key="index">
          <img :src="banner.image" v-lazy="banner.image" width="100%" />
        </van-swipe-item>
      </van-swipe>
    </div>
    <!-- type-bar -->
    <div class="type-bar">
      <div v-for="(cate, index) in category" :key="index">
        <img :src="cate.image" v-lazy="cate.image" width="90%" />
        <span>{{ cate.mallCategoryName }}</span>
      </div>
    </div>
    <!--AD banner area-->
    <div class="ad-banner">
      <img
        :src="adBanner.PICTURE_ADDRESS"
        v-lazy="adBanner.PICTURE_ADDRESS"
        width="100%"
      />
    </div>
    <!--Recommend goods area-->
    <div class="recommend-area">
      <div class="recommend-title">商品推荐</div>
      <div class="recommend-body"></div>
    </div>
    <!--swiper-->
    <swiper>
      <swiper-slide v-for="(item, index) in recommendGoods" :key="index">
        <div class="recommend-item">
          <img :src="item.image" width="80%" />
          <div>{{ item.goodsName }}</div>
          <div>￥{{ item.price }} (￥{{ item.mallPrice }})</div>
        </div>
      </swiper-slide>
    </swiper>
    <div>{{ msg }}</div>
    <!--floor one area-->
    <div class="floor">
      <div class="floor-anomaly">
        <div class="floor-one">
          <img :src="floor1_0.image" width="100%" />
        </div>
        <div>
          <div class="floor-two">
            <img :src="floor1_1.image" width="100%" />
          </div>
          <div>
            <img :src="floor1_2.image" width="100%" />
          </div>
        </div>
      </div>
      <div class="floor-rule">
        <div v-for="(item, index) in floor1.slice(3)" :key="index">
          <img :src="item.image" width="100%" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { Swiper, SwiperSlide, directive } from "vue-awesome-swiper";
import "swiper/swiper-bundle.css";

export default {
  name: "ShoppingMall",
  components: {
    Swiper,
    SwiperSlide,
  },
  directives: {
    swiper: directive,
  },
  data() {
    return {
      msg: "Shopping Mall",
      locationIcon: require("../assets/images/search.svg"),
      bannerPicArray: [
        {
          imageUrl:
            "https://aocoin-cdn.oss-cn-beijing.aliyuncs.com/assets/aolinkio/h5/static/img/wallet-banner.3db32.png",
        },
        {
          imageUrl:
            "https://aocoin-cdn.oss-cn-beijing.aliyuncs.com/assets/aolinkio/h5/static/img/staking-banner.d4262.png",
        },
        {
          imageUrl:
            "https://aocoin-cdn.oss-cn-beijing.aliyuncs.com/assets/aolinkio/h5/static/img/ksm-banner.7104f.png",
        },
      ],
      category: [],
      adBanner: {
        PICTURE_ADDRESS: null,
      },
      recommendGoods: [],
      floor1: [],
      floor1_0: { image: "" },
      floor1_1: { image: "" },
      floor1_2: { image: "" },
    };
  },
  created() {
    axios({
      url:
        "https://www.fastmock.site/mock/7ca1d4602a3b1864289829825b58d041/shop/api/smilevue",
      method: "get",
    })
      .then((response) => {
        console.log(response);
        if (response.status == 200) {
          this.category = response.data.data.category;
          this.adBanner = response.data.data.advertesPicture;
          this.bannerPicArray = response.data.data.slides;
          this.recommendGoods = response.data.data.recommend;
          this.floor1 = response.data.data.floor1; //楼层1数据
          this.floor1_0 = this.floor1[0];
          this.floor1_1 = this.floor1[1];
          this.floor1_2 = this.floor1[2];
        }
      })
      .catch((error) => {
        console.log(error);
      });
  },
};
</script>

<style lang="scss" scoped>
.search-bar {
  height: 2.2rem;
  background-color: #e5017d;
  line-height: 2.2rem;
}
.search-input {
  width: 100%;
  height: 1.3rem;
  border-top: 0px;
  border-left: 0px;
  border-right: 0px;
  border-bottom: 1px solid 1px !important ;
  background-color: #e5017d;
  color: #fff;
}
.location-icon {
  padding-top: 0.2rem;
  padding-left: 0.3rem;
}
.swiper-area {
  width: 20rem;
  clear: both;
  overflow: hidden;
}
.type-bar {
  background-color: #fff;
  margin: 0 0.3rem 0.3rem 0.3rem;
  border-radius: 0.3rem;
  font-size: 14px;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
}
.type-bar div {
  padding: 0.3rem;
  font-size: 12px;
  text-align: center;
}
.ad-banner {
  img {
    display: block;
    width: 100%;
  }
}
.recommend-area {
  background-color: #fff;
  margin-top: 0.3rem;
}
.recommend-title {
  border-bottom: 1px solid #eee;
  font-size: 14px;
  padding: 0.2rem;
  color: #e5017d;
}
.recommend-body {
  border-bottom: 1px solid #eee;
}
.recommend-item {
  width: 99%;
  border-right: 1px solid #eee;
  font-size: 12px;
  text-align: center;
}
.floor-anomaly {
  display: flex;
  flex-direction: row;
  background-color: #fff;
  border-bottom: 1px solid #ddd;
}
.floor-anomaly div {
  width: 10rem;

  box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
.floor-one {
  border-right: 1px solid #ddd;
}
.floor-two {
  border-bottom: 1px solid #ddd;
}
.floor-rule {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  background-color: #fff;
}
.floor-rule div {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  width: 10rem;
  border-bottom: 1px solid #ddd;
}
.floor-rule div:nth-child(odd) {
  border-right: 1px solid #ddd;
}
</style>
