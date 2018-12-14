<template>
  <div class="home">
       <div class="mui-content" v-for="(item,index) in newLists" :key="index">
			<ul class="mui-table-view" style="margin-top: 0px;"   >
			    <p  style="padding: 0px  12px;font-size: 15px;color: #303030;">{{item.news_title}}</p>
               
                <p  style="padding: 0px  12px;font-size: 12px;"> {{item.news_date}} </p>
                <div class="ht2" style="padding: 0px 12px"></div>
			    <p  class="pad14" style="padding: 12px;font-size: 12px;">{{item.news_content}}</p>
			</ul>
	   </div>
        
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "newsDetail",
//   components: {
//     Carousel
//   },
  data() {
    return {
      //新闻内容
      newLists:{}
    };
  },

  mounted() {
    this.getIndexData();
  },
  methods: {

      
    // 接口服务
    getIndexData() {
     let _this = this;

    //新闻接扣
    axios({
            url: 'http://47.110.40.116/ycg/public/index.php/index/index/newsdetails',
            method: 'post',
            responseType: 'json', // 默认的
            data: {news_id:_this.$route.query.id}
        }).then(function (res) {
          let newList=  JSON.stringify(res);                
          console.log(JSON.parse(newList).data);
          _this.newLists=JSON.parse(newList).data;     
          console.log(_this.newLists.news_title);
        }).catch(function (error) {
            console.log(error);
        })
    },
   
    // 路由服务
    godb(){
    //   this.$router.push({
    //           path: "/classify",
    //           query: {
    //             id: id
    //           }
    //         });
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