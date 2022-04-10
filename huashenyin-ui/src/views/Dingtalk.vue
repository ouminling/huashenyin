<template>
  <div class="register">
    <el-form ref="registerForm" :model="registerForm" :rules="registerRules" class="register-form">
      <h3 class="title">花神引后台管理系统</h3>



        <div id="Dingtalk_QRcode">
          <div id="login_container"></div>
        </div>
<!--        <div class="QRcode">
          <img  src="../assets/images/QRcode.png">
        </div>-->
      <div class="text">登录视为您已同意
        <el-link :underline="false" style="font-size:13px;vertical-align:0px;" type="primary" href="https://cdn.jsdelivr.net/gh/ouminling/CDN/花神引网站服务条款（www.huashenyin.cn）.pdf" target="_blank" class="protocol">
          服务条款</el-link>,
        <el-link :underline="false"  style="font-size:13px;vertical-align:0px" type="primary" href="https://cdn.jsdelivr.net/gh/ouminling/CDN/花神引法律声明及隐私权政策.pdf" target="_blank" class="protocol">
          隐私政策</el-link>
      </div>

        <div>
          <div  style=" margin: 10px auto 8px auto;">其他登录方式</div>

          <div class="icons-list">
            <el-tooltip class="item" effect="dark" content="钉钉扫码登录" placement="bottom">
              <el-link :underline="false" href="/Dingtalk"><svg-icon class="dingtalk anticon" icon-class="dingtalk" /></el-link>
            </el-tooltip>
            <el-tooltip class="item" effect="dark" content="暂未实现" placement="bottom">
              <el-link :underline="false" href=""><svg-icon class="WeChat anticon" icon-class="WeChat" /></el-link>
            </el-tooltip>
            <el-tooltip class="item" effect="dark" content="暂未实现" placement="bottom">
              <el-link :underline="false" href=""><svg-icon class="Alipay anticon" icon-class="Alipay" /></el-link>
            </el-tooltip>
            <el-tooltip class="item" effect="dark" content="暂未实现" placement="bottom">
              <el-link :underline="false" href=""><svg-icon class="Sina anticon" icon-class="Sina" /></el-link>
            </el-tooltip>


          </div>
        </div>
        <div style="float: right;">
          <el-link :underline="false" href="/login">使用已有账户登录</el-link>
        </div>
      </el-form>

    <!--  底部  -->
    <div class="el-register-footer">
      <span>Copyright © 2018-2022 huasheyin.link All Rights Reserved.</span>
    </div>
  </div>

</template>
<script>
import axios from "axios";
import { getCodeImg, register } from "@/api/login";
export default {
  name: "Dingtalk_QRcode",
  components: {},
  data() {
    return {
      appid: "ding78rmwx8qckrjjemm",
      redirectUrl: "https://www.cnblogs.com/huashenyin/",
      apiUrl:
        "<用户信息接口,把redirectUrl回调获取的CODE给后端用于获取扫码用户信息>",
      dingCodeConfig: {
        id: "login_container",
        style: "border:none;background-color:#FFFFFF;",
        width: "365",
        height: "400",
      },
    };
  },
  computed: {
    getRedirectUrl() {
      return encodeURIComponent(this.redirectUrl);
    },
    getAuthUrl() {
      return `https://oapi.dingtalk.com/connect/oauth2/sns_authorize?appid=${this.appid}&response_type=code&scope=snsapi_login&state=STATE&redirect_uri=${this.getRedirectUrl}`;
    },
    getGoto() {
      return encodeURIComponent(this.getAuthUrl);
    },
    getDingCodeConfig() {
      return { ...this.dingCodeConfig, goto: this.getGoto };
    },
  },
  created() {
    //调用
    this.initDingJs();
  },
  mounted() {
    this.addDingListener();
    this.initDingLogin();
    this.getUser();
  },
  methods: {
    initDingJs() {
      !(function (window, document) {
        function d(a) {
          var e,
            c = document.createElement("iframe"),
            d = "https://login.dingtalk.com/login/qrcode.htm?goto=" + a.goto;
          (d += a.style ? "&style=" + encodeURIComponent(a.style) : ""),
            (d += a.href ? "&href=" + a.href : ""),
            (c.src = d),
            (c.frameBorder = "0"),
            (c.allowTransparency = "true"),
            (c.scrolling = "no"),
            (c.width = a.width ? a.width + "px" : "365px"),
            (c.height = a.height ? a.height + "px" : "400px"),
            (e = document.getElementById(a.id)),
            (e.innerHTML = ""),
            e.appendChild(c);
        }

        window.DDLogin = d;
      })(window, document);
    },
    addDingListener() {
      let self = this;

      let handleLoginTmpCode = function (loginTmpCode) {
        window.location.href =
          self.getAuthUrl + `&loginTmpCode=${loginTmpCode}`;
      };

      let handleMessage = function (event) {
        if (event.origin == "https://login.dingtalk.com") {
          handleLoginTmpCode(event.data);
        }
      };

      if (typeof window.addEventListener != "undefined") {
        window.addEventListener("message", handleMessage, false);
      } else if (typeof window.attachEvent != "undefined") {
        window.attachEvent("onmessage", handleMessage);
      }
    },
    initDingLogin() {
      window.DDLogin(this.getDingCodeConfig);
    },
    getUser() {
      let getQueryString = function (name) {
        var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
        var r = window.location.search.substr(1).match(reg);
        if (r != null) {
          return unescape(r[2]);
        }
        return null;
      };

      let code = getQueryString("code");

      if (code !== null) {
        axios
          .get(`${this.apiUrl}?code=${code}`)
          .then((response) => {
            console.log(response);
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
    handleRegister() {
      this.$refs.registerForm.validate(valid => {
        if (valid) {
          this.loading = true;
          register(this.registerForm).then(res => {
            const username = this.registerForm.username;
            this.$alert("<font color='red'>恭喜你，您的账号 " + username + " 注册成功！</font>", '系统提示', {
              dangerouslyUseHTMLString: true,
              type: 'success'
            }).then(() => {
              this.$router.push("/login");
            }).catch(() => {});
          }).catch(() => {
            this.loading = false;
            if (this.captchaOnOff) {
              this.getCode();
            }
          })
        }
      });
    }
  },
};
</script>

<style rel="stylesheet/scss" lang="scss">
#Dingtalk_QRcode {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  login_container{
    width: 50%;
  }



}
 .text{
        margin-top: -100px;
        font-size: 13px;
        text-align: center;

      }

.register {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  background-image: url("../assets/images/login-background.jpg");
  background-size: cover;
}
.title {
  margin: 0px auto -20px auto;
  text-align: center;
  color: #707070;
}

.register-form {
  border-radius: 6px;
  background: rgba(255, 255, 255, );
  width: 400px;
  padding: 25px 25px 5px 25px;
}


  .el-register-footer {
    height: 40px;
    line-height: 40px;
    position: fixed;
    bottom: 0;
    width: 100%;
    text-align: center;
    color: #fff;
    font-family: Arial;
    font-size: 12px;
    letter-spacing: 1px;
  }

  .QRcode {

    display: flex;
    justify-content: center;
    align-items: center;

    img {
      width: 50%;
    }
  }

  .icons-list {
    .anticon {
      font-size: 30px;
      width: 40px;
    }

    .dingtalk {
      color: #0089FF;
    }

    .WeChat {
      color: #51C332;
    }

    .Alipay {
      color: #06B4FD;
    }

    .Sina {
      color: #D81E06;
    }
  }

</style>
