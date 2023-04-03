<template>
  <header>
    <nav class="navbar navbar-expand-lg   bg-dark">
      <div class="container">
        <a class="navbar-brand" href="#">
          <i class="ace-icon fa fa-video-camera"></i>&nbsp;大连旅行
        </a>
        <el-button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent"
                 aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </el-button>

        <el-menu class="collapse navbar-collapse" id="navbarSupportedContent">
<!--          <ul class="navbar-nav mr-auto">-->
            <el-menu-item class="nav-item active">
              <router-link class="nav-link" to="/">主页</router-link>
            </el-menu-item>
            <el-menu-item class="nav-item active">
              <router-link class="nav-link" to="/list">全部分类</router-link>
            </el-menu-item>
            <el-menu-item class="nav-item dropdown active">
              <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                更多
              </a>
              <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                <el-row gutter="10">
                  <!-- 如果想在当前窗口跳转至新页面   <div onclick="window.open('url')"> </div>-->
                  <el-col :span="4"><div class="dropdown-item" href="#" onclick="window.open('https://chen-boran.github.io/')">关于我们</div></el-col>
                </el-row>
                <el-row gutter="10">
                  <el-col :span="4"><div class="dropdown-item" href="#">渠道合作</div></el-col>
                </el-row>
                <el-row gutter="10">
                  <div class="dropdown-divider"></div>
                </el-row>
                <el-row gutter="10">
                  <el-col :span="4"><div class="dropdown-item" href="#">更多信息</div></el-col>
                </el-row>
              </div>
            </el-menu-item>
<!--          </ul>-->
          <span v-show="loginMember.id" class="text-white pr-3" @click="logout">您好：{{loginMember.name}}</span>
          <button v-show="loginMember.id" @click="logout()" class="btn btn-outline-light my-2 my-sm-0">退出登录</button>
          <button v-show="!loginMember.id" @click="openLoginModal()" class="btn btn-outline-light my-2 my-sm-0">登录/注册</button>
        </el-menu>
      </div>
    </nav>

    <the-login ref="loginComponent"></the-login>
  </header>
</template>

<script>

  import TheLogin from "./login";
  export default {
    name: 'theHeader',
    components: {TheLogin},
    data: function () {
      return {
        loginMember: {}
      }
    },
    mounted() {
      let _this = this;
      _this.loginMember = Tool.getLoginMember();
    },
    methods: {
      /**
       * 打开登录注册窗口
       */
      openLoginModal() {
        let _this = this;
        _this.$refs.loginComponent.openLoginModal();
      },

      setLoginMember(loginMember) {
        let _this = this;
        _this.loginMember = loginMember;
      },

      logout () {
        let _this = this;
        _this.$ajax.get(process.env.VUE_APP_SERVER + '/business/web/member/logout/' + _this.loginMember.token).then((response)=>{
          let resp = response.data;
          if (resp.success) {
            Tool.setLoginMember(null);
            _this.loginMember = {};
            Toast.success("退出登录成功");
            _this.$router.push("/");
          } else {
            Toast.warning(resp.message);
          }
        });
      },

    }
  }
</script>
