<template>
  <main role="main">
    <section class="jumbotron text-center">
      <el-carousel :interval="4000" @change="changeImg" style="width: 100%;margin-top: 1px;" trigger="click"
                   height="43.5rem">
        <el-carousel-item v-for="item  in bannerList" :key="item">
          <el-image
              :class="className"
              style="width: 100%; height: 100%"
              :src="item"
              fit="cover"></el-image>
        </el-carousel-item>
      </el-carousel>
      <div class="container">
        <h1>大连旅行分享平台</h1>
        <h2>
          TravelSharingPlatform</h2>
        <p class="lead text-muted m-3">
          美丽的自然风光，大连还有独特的文化底蕴，这里曾是多个国家殖民和占领的历史见证。旅顺口炮台，感受历史的厚重感；日俄战争博物馆，历史的沧桑巨变。
        </p>
        <p>
          <router-link to="/list" class="btn btn-primary my-2 p-3 font-weight-bold">点击进入所有课程</router-link>
        </p>
      </div>
    </section>

    <div class="album py-5 bg-light">
      <div class="container">
        <div class="title1">最新发布</div>
        <div class="row">
          <div v-for="o in news" class="col-md-4">
            <the-course v-bind:course="o"></the-course>
          </div>
        </div>

        <hr>

        <div class="title1">好玩推荐</div>
        <div class="row">
          <div v-for="o in news" class="col-md-4">
            <the-course v-bind:course="o"></the-course>
          </div>
        </div>
      </div>
    </div>

  </main>
</template>

<script>

  import TheCourse from "../components/the-course";
  export default {
    name: 'index',
    components: {TheCourse},
    mounted () {
      let _this = this;
      _this.listNew();
      this.className = "lun-img";
      setTimeout(() => {
        this.className = "lun-img-two";
      }, 300);
    },
    methods: {
      /**
       * 查询新上好课
       */
      listNew() {
        let _this = this;

        // 新上好课不经常变，又经常被访问，适合用缓存
        // 判断是否有缓存
        let news = SessionStorage.get("news");
        if (!Tool.isEmpty(news)) {
          _this.news = news;
          return;
        }

        _this.$ajax.get(process.env.VUE_APP_SERVER + '/business/web/course/list-new').then((response)=>{
          console.log("查询新上好课结果：", response);
          let resp = response.data;
          if (resp.success) {
            _this.news = resp.content;
            // 保存到缓存
            SessionStorage.set("news", _this.news);
          }
        }).catch((response)=>{
          console.log("error：", response);
        })
      },
      //轮播图切换
      changeImg(e) {
        // console.log(e, "当前下标");
        this.className = "lun-img";
        setTimeout(() => {
          this.className = "lun-img-two";
        }, 300);
      },
    },
    data() {
      var img1 = require("../assets/img1.jpg");
      var img2 = require("../assets/img2.jpg");
      var img3 = require("../assets/img3.jpg");
      return {
        bannerList: [img1,img2,img3],//轮播图地址 这块写你们的轮播图数组即可
        className: ""//轮播图名字
      //   require("../assets/img1.jpg"),
      // require("../assets/img2.jpg"),
      //     require("../assets/img3.jpg")
      };
    }
  }
</script>

<style>
  .title1{
    margin-bottom: 2rem;
    color: #fafafa;
    letter-spacing: 0;
    text-shadow: 0px 1px 0px #999, 0px 2px 0px #888, 0px 3px 0px #777, 0px 4px 0px #666, 0px 5px 0px #555, 0px 6px 0px #444, 0px 7px 0px #333, 0px 8px 7px #001135;
    font-size: 2rem;
  }
  .title2{
    margin-bottom: 2rem;
    color: transparent;
    -webkit-text-stroke: 1px black;
    letter-spacing: 0.04em;
    font-size: 2rem;
  }
  .el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 200px;
    margin: 0;
  }

  .el-carousel__item:nth-child(2n) {
    background-color: #99a9bf;
  }

  .el-carousel__item:nth-child(2n+1) {
    background-color: #d3dce6;
  }
  .carousel-map {
    width: 100%;
    height: 43.5rem;
    overflow: hidden;
  }
  .lun-img {
      transform: scale(1.5);
  }
  .lun-img-two {
      transition: all 3s;
      transform: scale(1);
  }
  .el-carousel__item.is-animating {
      transition: all 0.6s;
  }
</style>
