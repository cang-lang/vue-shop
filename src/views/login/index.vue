<template>
  <div class="login">
    <div class="login-box">
      <!-- 头像区域 -->
      <div class="avatar-box">
        <img src="./头像.jpg" alt="">
      </div>
      <!-- 登录表单区域 -->
      <el-form ref="logFormRef" class="login-form" :model="loginForm" :rules="loginFormRules" label-width="0px" >
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input  v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item  prop="password">
          <el-input  v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
        </el-form-item>
        <!-- 按钮区域 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>

    </div>
  </div>
</template>
<script>
export default {
  data () {
    return {
      // 这是表单绑定数据对象
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      // 这是表单验证规则对象
      loginFormRules: {
        // 验证用户名
        username: [
          { required: true, message: '请输入登录名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        // 验证密码
        password: [
          { required: true, message: '请输入登录密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 点击重置按钮，重置表单
    resetLoginForm () {
      this.$refs.logFormRef.resetFields()
    },
    login () {
      this.$refs.logFormRef.validate(async (valid) => {
        if (!valid) return
        const { data: { data, meta } } = await this.$http.post('login', this.loginForm)
        // console.log(data, meta)
        if (meta.status !== 200) return this.$message.error(meta.msg)
        this.$message.success(meta.msg)
        // console.log(data)
        // 存储token到session中
        window.sessionStorage.setItem('token', data.token)
        this.$router.push('home')
      })
    }
  }
}
</script>
<style lang="less" scoped>
  .login{
    height: 100%;
    background:url('./背景图.jpg') no-repeat;
    background-position: center;

  .login-box{
      width: 450px;
      height: 300px;
      background-color: #43647c66;
      border-radius: 3px;
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);

  .avatar-box {
      height: 130px;
      width: 130px;
      border: 1px solid #eee;
      border-radius: 50%;
      padding: 10px;
      box-shadow: 0 0 10px #ddd;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #fff;
      img {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #eee;
      }
  }

   }

}

  .login-form {
      position: absolute;
      bottom: 0;
      width: 100%;
      padding: 0 20px;
      box-sizing: border-box;
    }

    .btns {
      display: flex;
      justify-content: flex-end;
    }
</style>
