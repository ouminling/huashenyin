<template>
  <div class="register">
    <el-form ref="registerForm" :model="registerForm" :rules="registerRules" class="register-form">
      <h3 class="title">花神引后台管理系统</h3>
      <el-form-item>
        <div class="text" >使用QQ/支付宝/钉钉扫码快速登录</div>
        <div class="QRcode">
        <img  src="../assets/images/QRcode.png">
        </div>



        <div class="text">登录视为您已同意
          <el-link :underline="false" style="font-size:12px" type="primary" href="https://cdn.jsdelivr.net/gh/ouminling/CDN/花神引网站服务条款（www.huashenyin.cn）.pdf" target="_blank" class="protocol">
            服务条款</el-link>,
          <el-link :underline="false"  style="font-size:12px" type="primary" href="https://cdn.jsdelivr.net/gh/ouminling/CDN/花神引法律声明及隐私权政策.pdf" target="_blank" class="protocol">
            隐私政策</el-link>
        </div>
        <div>
          <div  style=" margin-top: 0px;">其他登录方式</div>

          <div class="icons-list">
            <el-link :underline="false" href="https://oapi.dingtalk.com/connect/qrconnect?appid=ding78rmwx8qckrjjemm&response_type=code&scope=snsapi_login&state=STATE&redirect_uri=https://ding-doc.dingtalk.com/"><svg-icon class="dingtalk anticon" icon-class="dingtalk" /></el-link>
            <el-link :underline="false" href="https://www.cnblogs.com/huashenyin/"><svg-icon class="WeChat anticon" icon-class="WeChat" /></el-link>
            <el-link :underline="false" href="https://www.cnblogs.com/huashenyin/"><svg-icon class="Alipay anticon" icon-class="Alipay" /></el-link>
            <el-link :underline="false" href="https://www.cnblogs.com/huashenyin/"><svg-icon class="Sina anticon" icon-class="Sina" /></el-link>

          </div>
        </div>
        <div style="float: right;">
          <router-link class="link-type" :to="'/login'">使用已有账户登录</router-link>
        </div>
      </el-form-item>
    </el-form>
    <!--  底部  -->
    <div class="el-register-footer">
      <span>Copyright © 2018-2022 huasheyin.link All Rights Reserved.</span>
    </div>
  </div>
</template>

<script>
import { getCodeImg, register } from "@/api/login";

export default {
  name: "Register",
  data() {
    const equalToPassword = (rule, value, callback) => {
      if (this.registerForm.password !== value) {
        callback(new Error("两次输入的密码不一致"));
      } else {
        callback();
      }
    };
    return {
      codeUrl: "",
      registerForm: {
        username: "",
        password: "",
        confirmPassword: "",
        code: "",
        uuid: ""
      },
      registerRules: {
        username: [
          { required: true, trigger: "blur", message: "请输入您的账号" },
          { min: 2, max: 20, message: '用户账号长度必须介于 2 和 20 之间', trigger: 'blur' }
        ],
        password: [
          { required: true, trigger: "blur", message: "请输入您的密码" },
          { min: 5, max: 20, message: '用户密码长度必须介于 5 和 20 之间', trigger: 'blur' }
        ],
        confirmPassword: [
          { required: true, trigger: "blur", message: "请再次输入您的密码" },
          { required: true, validator: equalToPassword, trigger: "blur" }
        ],
        code: [{ required: true, trigger: "change", message: "请输入验证码" }]
      },
      loading: false,
      captchaOnOff: false
    };
  },
  created() {
    this.getCode();
  },
  methods: {
    getCode() {
      getCodeImg().then(res => {
        this.captchaOnOff = res.captchaOnOff === undefined ? true : res.captchaOnOff;
        if (this.captchaOnOff) {
          this.codeUrl = "data:image/gif;base64," + res.img;
          this.registerForm.uuid = res.uuid;
        }
      });
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
  }
};
</script>

<style rel="stylesheet/scss" lang="scss">
.text{
  font-size: 12px;
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
  margin: 0px auto 30px auto;
  text-align: center;
  color: #707070;
}

.register-form {
  border-radius: 6px;
  background: rgba(255, 255, 255, .3);
  width: 400px;
  padding: 25px 25px 5px 25px;
  .el-input {
    height: 38px;
    input {
      height: 38px;
    }
  }
  .input-icon {
    height: 39px;
    width: 14px;
    margin-left: 2px;
  }
}
.register-tip {
  font-size: 13px;
  text-align: center;
  color: #bfbfbf;
}
.register-code {
  width: 33%;
  height: 38px;
  float: right;
  img {
    cursor: pointer;
    vertical-align: middle;
  }
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
.register-code-img {
  height: 38px;
}
.QRcode{

    display: flex;
    justify-content: center;
    align-items: center;

 img {
    width: 50%;
  }
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
