<template>
<div class="wrapper">
  <div style="margin: 200px auto; background-color: #fff; width: 350px ; height: 300px; padding: 20px; border-radius: 10px">
    <div style="margin: 20px 0; text-align: center; font-size: 24px;"><b>登录</b></div>
    <el-form :model="Account" :rules="rules" ref="userForm">
      <el-form-item prop="account">
        <el-input  size="medium" style="margin: 10px 0" prefix-icon="el-icon-user" v-model="Account.account"></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input  size="medium" style="margin: 10px 0" prefix-icon="el-icon-lock" show-password v-model="Account.password"></el-input>
      </el-form-item>

      <el-form-item style="margin: 10px 0; text-align: right">
        <el-button type="primary" size="small" autocomplete="off" @click="login">登录</el-button>
        <el-button type="warning" size="small" autocomplete="off" @click="$router.push('/register')">注册</el-button>
      </el-form-item>
    </el-form>
  </div>
</div>
</template>

<script>
export default {
  name: "Login",
  data(){
    return{
      Account:{},
      rules: {
        account: [
          {required: true, message: '请输入用户名', trigger: 'blur'},
          {min: 1, max: 15, message: '长度在 1 到 15 个字符', trigger: 'blur'}
        ],
        password: [
          {required: true, message: '请输入密码', trigger: 'blur'},
          {min: 1, max: 25, message: '长度在 1 到 20 个字符', trigger: 'blur'}
        ],
      }
    }
  },
  methods: {
    login(){
      this.$refs["userForm"].validate((valid) => {
        if (valid) {
          this.request.post("/user/login",this.Account).then(res => {
            if(!res){
              this.$message.error("用户密码错误")
            }else {
              this.$router.push("/")
            }
          })
        } else {
          return false;
        }
      });
    }
  }
}
</script>

<style scoped>
.wrapper{
  height: 100vh;
  background: url("~@/static/img/img.png");
  overflow: hidden;
}
</style>