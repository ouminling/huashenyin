<template>
  <div class="login">
    <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login-form">
      <h3 class="title">花神引管理系统</h3>
      <el-form-item prop="username">
        <el-input
          v-model="loginForm.username"
          type="text"
          auto-complete="off"
          placeholder="账号"
        >
          <svg-icon slot="prefix" icon-class="user" class="el-input__icon input-icon" />
        </el-input>
      </el-form-item>

      <el-form-item prop="password">
        <el-input
          v-model="loginForm.password"
          type="password"
          auto-complete="off"
          placeholder="密码"
          @keyup.enter.native="handleLogin"
        >
          <svg-icon slot="prefix" icon-class="password" class="el-input__icon input-icon" />
        </el-input>
      </el-form-item>
      <Verify
        @success="capctchaCheckSuccess"
        :mode="'pop'"
        :captchaType="'blockPuzzle'"
        :imgSize="{ width: '330px', height: '155px' }"
        ref="verify"
      ></Verify>




      <div >
      <el-checkbox v-model="loginForm.rememberMe" style="margin:0 0 25px 0;" >记住密码</el-checkbox>
        <el-link :underline="false" type="primary" style="float: right;line-height: 20px">忘记密码</el-link>


      </div>




      <el-form-item style="width: 100%">
        <el-button
          :loading="loading"
          size="medium"
          type="primary" round
          style="width:100%;"
          @click.native.prevent="handleLogin"
          @click="login_welcome"
        >
          <span v-if="!loading">登 录</span>
          <span v-else>登 录 中...</span>
        </el-button>


        <div  style="float: left;line-height: 30px" v-if="register">
          <h5><router-link class="link-type" :to="'/register'" >立即注册</router-link></h5>
        </div>

        <div  style="float: right;line-height: 30px" >

          <h5><router-link class="link-type" :to="'/QRcode'" >二维码登录</router-link></h5>
        </div>


      </el-form-item>
    </el-form>


    <!--  底部  -->

    <div class="el-login-footer">
      <span>Copyright © 2021-2022 huashaenyin.link All Rights Reserved.</span>
    </div>

    <!-- 背景音乐 -->
    <audio src="../assets/audio/asoul.mp3"
           loop='true'
           autoplay="autoplay"
           ref="MusicPlay"
           hidden
    ></audio>

  </div>



</template>



<script>
import Cookies from "js-cookie";
import { encrypt, decrypt } from "@/utils/jsencrypt";
import Verify from "@/components/Verifition/Verify";
import { timeFix } from '@/utils/util'

export default {
  components: { Verify },
  name: "Login",
  data() {
    return {
      loginForm: {
        username: "",
        password: "",
        rememberMe: false,
        code: "",
      },
      loginRules: {
        username: [
          { required: true, trigger: "blur", message: "请输入您的账号" }
        ],
        password: [
          { required: true, trigger: "blur", message: "请输入您的密码" }
        ],
      },
      loading: false,
      // 注册开关
      register: true,
      redirect: undefined
    };
  },
  watch: {
    $route: {
      handler: function (route) {
        this.redirect = route.query && route.query.redirect;
      },
      immediate: true
    }
  },
  created() {
    this.getCookie();
  },
  methods: {
    getCookie() {
      const username = Cookies.get("username");
      const password = Cookies.get("password");
      const rememberMe = Cookies.get("rememberMe");
      this.loginForm = {
        username: username === undefined ? this.loginForm.username : username,
        password: password === undefined ? this.loginForm.password : decrypt(password),
        rememberMe: rememberMe === undefined ? false : Boolean(rememberMe)
      };
    },
    capctchaCheckSuccess(params) {
      this.loginForm.code = params.captchaVerification;
      this.loading = true;
      if (this.loginForm.rememberMe) {
        Cookies.set("username", this.loginForm.username, { expires: 30 });
        Cookies.set("password", encrypt(this.loginForm.password), { expires: 30, });
        Cookies.set("rememberMe", this.loginForm.rememberMe, { expires: 30 });
      } else {
        Cookies.remove("username");
        Cookies.remove("password");
        Cookies.remove("rememberMe");
      }
      this.$store.dispatch("Login", this.loginForm).then(() => {
        this.$router.push({ path: this.redirect || "/" }).catch(() => {});
      }).catch(() => {
        this.loading = false;
      });
    },
    handleLogin() {
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          this.$refs.verify.show();
        }
      });
    },
    login_welcome() {

      this.$router.push({ path: '/' })

      setTimeout(() => {
        // 延迟 8 秒显示欢迎信息
        this.$notify.success({
          iconClass:'el-icon-s-opportunity',
          title: '欢迎',
          message: `${timeFix()}，欢迎回来`

        })
      },8000)

    },
  },

};

</script>

<style rel="stylesheet/scss" lang="scss">

.login {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  background-image: url("../assets/images/login-background.jpg");
  background-size: cover;
}
.title {
  margin: 0 auto 30px auto;
  text-align: center;
  color: #476B9B;
}

.login-form {
  border-radius: 6px;
  background: rgba(255, 255, 255, .3);
  width: 300px;
  height: auto;
  padding: 25px 25px 5px 25px;
  .el-input {
    height: 38px;
    input {
      height: 38px;
    }
  }
  .input-icon {
    height: 41px;
    width: 16px;
    margin-left: 2px;

  }
}
.login-tip {
  font-size: 13px;
  text-align: center;
  color: #bfbfbf;
}
.login-code {
  width: 33%;
  height: 38px;
  float: right;
  img {
    cursor: pointer;
    vertical-align: middle;
  }
}
.el-login-footer {
  height: 40px;
  line-height: 40px;
  position: fixed;
  bottom: 0;
  width: 100%;
  text-align: center;
  color: #fff;
  font-family: Arial;
  font-size: 14px;
  letter-spacing: 1px;
}
.login-code-img {
  height: 38px;
}
.icons-list {
  .anticon{
  font-size: 30px;
  width: 40px;
  }

  .dingtalk{
    color: #0089FF;
  }
  .WeChat{
    color: #51C332;
  }
  .Alipay{
    color: #06B4FD;
  }
  .Sina{
    color: #D81E06;
  }
}
</style>
