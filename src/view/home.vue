<template>
  <div class="home">
    <!-- 头部 -->
    <div class="myHeader">
      <div class="left">
        <img class="logo" src="@/assets/images/logo.png" />
        <span class="title">Hxx-Luxury Store</span>
      </div>
      <div class="right" @click="gotoPersonalCenter">
        <span class="iconfont">&#xe6b8;</span>
      </div>
    </div>
    <!-- 轮播 -->
    <carousel :swiperData='swiperData'></carousel>
    <!-- 分类 -->
    <div class="classification">
      <div class="mui-card" style="margin:auto">
        <ul class="mui-table-view mui-grid-view mui-grid-9">
          <li class="mui-table-view-cell mui-media mui-col-xs-3 mui-col-sm-4" id="wd20" v-for="(item,index) in classifiData" :key="index" @click="gotoClassify(item.lanmu_id)">
            <a href="#">
              <span id="top5">   <img :src="item.lanmu_url"  width="30px" height="25px"></span>
              <div class="mui-media-body" id="color30">{{item.lanmu_name}}</div>
            </a> 
          </li>
          </ul>
      </div>
    </div>
    <!-- 头条 -->
    <div class="recommend">
      <div class="title"> &nbsp; <img class="" src="@/assets/images/tt@2x.png" width="14px" height="20px" />&nbsp;<span class="cls">今日头条</span></div>
      <div class="content">
        <ul class="news">
            <li class="list list-right-img" v-for="(item,index) in newLists" :key="index" @click="goNewsDetail(item.news_id)">
              <div class="left">
                <div class="new">
                  <p class="title list-ellipsis-1">{{item.news_title}}</p>
                  <p class="content">{{item.news_content}}</p>
                </div>
              </div>
              <div class="right">
                <img :src="item.news_url" alt="">
              </div>
            </li>
        </ul>
        <div class="left" @click="godb()">
          <img class="logo" src="@/assets/images/home_db.png" width="100%" />
        </div>
      </div>
    </div>
    <div class="hit60"></div>
      <!-- 底部 -->
    <div class="mui-card" style="margin:auto;bottom: 0px;position: fixed; width: 100%;">
      <ul class="mui-table-view mui-grid-view mui-grid-9">
        <li class="mui-table-view-cell mui-media mui-col-xs-3 mui-col-sm-4" id="wd25">
          <a href="#" id="pad">
            <span id="top5"><img src="@/assets/images/sy@2x.png" width="20px"  height="20px"/></span>
            <div class="mui-media-body" id="color30">首页</div>
          </a>
        </li>
        <li class="mui-table-view-cell mui-media mui-col-xs-3 mui-col-sm-4" id="wd25">
          <a href="#" id="pad">
            <span id="top5"><img src="@/assets/images/fl@2x.png" width="20px"  height="20px"/></span>
            <div class="mui-media-body" id="color30">分类</div>
          </a>
        </li>
        <li class="mui-table-view-cell mui-media mui-col-xs-3 mui-col-sm-4" id="wd25">
          <a href="#" id="pad">
            <span id="top5"><img src="@/assets/images/xx@2x.png" width="20px"  height="20px"/></span>
            <div class="mui-media-body" id="color30">消息</div>
          </a>
        </li>
        <li class="mui-table-view-cell mui-media mui-col-xs-3 mui-col-sm-4" id="wd25">
          <a href="#" id="pad">
            <span id="top5"><img src="@/assets/images/wd@2x.png" width="20px"  height="20px"/></span>
            <div class="mui-media-body" id="color30">我的</div>
          </a>
        </li>
      </ul>
    </div>

  </div>
</template>
<script>
import Carousel from "@/components/carousel";
import axios from "axios";
export default {
  name: "home",
  components: {
    Carousel
  },
  data() {
    return {
      // 轮播图
      swiperData: [],
      // 分类
      classifiData: {},
      //
      newLists:{},
      // 推荐
      recommendData: []
    };
  },
  mounted() {
    this.getIndexData();
  },
  methods: {
    // 接口服务
    getIndexData() {
     let _this = this;
     //轮播接扣
    axios({
            url: 'http://47.110.40.116/ycg/public/index.php/index/index/lunbolist',
            method: 'post',
            responseType: 'json', // 默认的
            data: {}
        }).then(function (res) {    
           let lunbos=  JSON.stringify(res);
        for(var i in lunbos){
            _this.swiperData.push(JSON.parse(lunbos).data.data[i].lunbo_url);
        }
        }).catch(function (error) {
            console.log(error);
        })
         
         //分类接扣
    axios({
            url: 'http://47.110.40.116/ycg/public/index.php/index/index/protypelist',
            method: 'post',
            responseType: 'json', // 默认的
            data: {}
        }).then(function (res) {  
          let classifs=  JSON.stringify(res);                
          console.log(JSON.parse(classifs).data.data);
          _this.classifiData=JSON.parse(classifs).data.data;     
        }).catch(function (error) {
            console.log(error);
        })

        //新闻接扣
    axios({
            url: 'http://47.110.40.116/ycg/public/index.php/index/index/newslist',
            method: 'post',
            responseType: 'json', // 默认的
            data: {}
        }).then(function (res) {
          let newList=  JSON.stringify(res);                
          console.log(JSON.parse(newList).data.data);
          _this.newLists=JSON.parse(newList).data.data;     
        }).catch(function (error) {
            console.log(error);
        })
    },
   
    // 路由服务
    goNewsDetail(id){
      this.$router.push({
              path: "/newsDetail",
              query: {
                id: id
              }
            });
    },
    gotoClassify(id) {
      this.$router.push({
        path: "/classify",
        query: {
          id: id
        }
      });
    },
    gotoPersonalCenter() {
       this.$router.push({
        path: "/me"
      });
    }
  }
};
</script>
<style lang="less" scoped>

.home {
  background: #f3f4f5;
  // 头部
  .myHeader {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    height: 1.066667rem;
    padding: 0 0.133333rem;
    background: #fff;
    .left {
      display: flex;
      flex-direction: row;
      align-items: center;
      .logo {
        width: 0.8rem;
        height: 0.8rem;
      }
      .title {
        padding-left: 0.133333rem;
        font-size: 0.4rem;
        color: #8e8e8e;
      }
    }
    .right {
      span {
        font-size: 0.5rem;
      }
    }
  }
  // 分类
  .classification {
    .class_top,
    .class_bottom {
      display: flex;
      flex-direction: row;
      justify-content: space-around;
      .item {
        img {
          display: block;
          width: 2rem;
          height: 2.346667rem;
        }
      }
    }
  }
  // 推荐
  .recommend {
    .title {
      text-align: left;
      font-size: 0.373333rem;
      padding: 10px 0px;
      background: #fff;
      margin: 0.133333rem 0;
    }
    .content {
      .item {
        background: #fff;
        margin-bottom: 0.133333rem;
        img {
          width: 100%;
          height: 4.8rem;
          display: block;
        }
        img[lazy="loading"] {
          display: block;
          width: 50px;
          height: 50px;
          margin: 0 auto;
        }
        .item_text {
          box-sizing: border-box;
          display: flex;
          flex-direction: row;
          justify-content: space-between;
          padding: 0.133333rem;
          font-size: 0.32rem;
        }
      }
    }
  }
}

/* 新闻 */
.cls{
      position: relative;
    top: 2px;
    color: #00A8FF;
}
.news {
    background: #fff;
}

.news .list {
    display: flex;
    display: -webkit-flex;
    padding: 10px 5px 5px;
    margin: 0 15px;
    border-bottom: 1px solid #ddd;
}

.news .list:last-child {
    border: 0;
}


/* 右图模式 */

.list-right-img .left {
    width:75%;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    justify-content: space-between;
}

.list-right-img .left .title {
    font-weight: bold;
    height: 30%;
    font-size: 14px;
}

.list-right-img .left .content {
    font-size: 12px;
    color: #888;
    margin: 5px 0;
    display: -webkit-box;
    overflow: hidden;
    text-overflow: ellipsis;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    line-height: 16px;
}

.list-right-img .left .time {
    font-size: 10px;
    color: #bbb;
}

.list-right-img .right {
    width: 25%;
    margin-left: 10px;
}

.list-right-img .right img {
    width: 75px;
    height: 75px;
}

/* 右图模式结束 */




/* 其他 */

/* 单行省略 */
.list-ellipsis-1 {
    text-align: left;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
}



</style>