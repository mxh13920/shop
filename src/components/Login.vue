<template>
  <div class="login-background">
    <div class="login-box">
      <!--      logo-->
      <div class="avatar">
        <img src="../assets/logo.png" alt="">
      </div>
      <!--      表单-->
      <el-form ref="loginFormRef" :rules="loginFotmRules" :model="loginform" label-width="0px" class="login_form">
        <el-form-item prop="username">
          <el-input placeholder="请输入账号" v-model="loginform.username" prefix-icon="iconfont icon-user" clearable/>
        </el-form-item>
        <el-form-item prop="password">
          <el-input placeholder="请输入密码" v-model="loginform.password" show-password
                    prefix-icon="iconfont icon-lock_fill"/>
        </el-form-item>
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="restLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        // 表单登录数据
        loginform: {
          username: 'admin',
          password: '123456'
        },
        // 表单登录数据规则
        loginFotmRules: {
          username: [
            {
              required: true,
              message: '请输入账号',
              trigger: 'blur'
            },
            {
              min: 5,
              max: 16,
              message: '长度在 5 到 16 个字符',
              trigger: 'blur'
            }
          ],
          password: [
            {
              required: true,
              message: '请输入密码',
              trigger: 'blur'
            },
            {
              min: 3,
              max: 16,
              message: '长度在 3 到 16 个字符',
              trigger: 'blur'
            }
          ]
        }
      }
    },
    methods: {
      // 重置密码
      restLoginForm () {
        // console.log(this)
        this.$refs.loginFormRef.resetFields()
        this.loginform.username = ''
        this.loginform.password = ''
      },
      login () {
        this.$refs.loginFormRef.validate(async valid => {
          if (!valid) return
          const { data: res } = await this.$http.post('login', this.loginform)
          console.log(res)
          if (res.meta.status !== 200) return this.$message.error('登录失败')
          this.$message.success('登录成功')
          window.sessionStorage.setItem('token', res.data.token)
          this.$router.push('/home')
        })
      }
    }
  }
</script>

<style lang="less" scoped>
  .login-background {
    height: 100%;
    background-color: #294b69;

    .login-box {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 450px;
      height: 300px;
      background: #fff;
      border-radius: 10px;

      .avatar {
        position: absolute;
        left: 50%;
        transform: translate(-50%, -50%);
        width: 130px;
        height: 130px;
        border: 1px #ffffff solid;
        border-radius: 50%;
        padding: 10px;
        background-color: #fff;
        box-shadow: 0 0 10px #ddd;

        img {
          width: 100%;
          height: 100%;
          border-radius: 50%;
          background-color: #eee;
        }
      }
    }

    .login_form {
      position: absolute;
      bottom: 0;
      width: 100%;
      padding: 10px;
      box-sizing: border-box;

      .btns {
        display: flex;
        justify-content: flex-end;
      }
    }
  }

</style>
