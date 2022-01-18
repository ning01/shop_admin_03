<template>
  <div class="login">
    <el-form
      ref="form"
      status-icon
      :rules="rules"
      :model="form"
      label-width="80px"
    >
      <img src="@/assets/6.jpg" alt="" />
      <el-form-item label="用户名称" prop="username">
        <el-input v-model="form.username" placeholder="请输入用户名"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input
          v-model="form.password"
          @keyup.enter="loginform"
          placeholder="请输入密码"
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="login">登录</el-button>
        <el-button @click="reset">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      form: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          { required: true, message: '用户名不能为空', trigger: 'blur' },
          { min: 3, max: 9, message: '长度在3-9个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '密码不能为空', trigger: 'blur' },
          { min: 6, max: 12, message: '长度在6-12个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    reset () {
      this.$refs.form.resetFields()
    },
    login () {
      this.$refs.form.validate(valid => {
        // valid为true就表示通过,否则不通过
        if (valid) {
          // 发送ajsx请求,进行登录
          axios({
            method: 'post',
            url: 'http://127.0.0.1:8888/api/private/v1/login',
            data: this.form
          }).then(res => {
            console.log(res.data)
            if (res.data.meta.status === 200) {
              this.$message.success('登录成功')
              // 把后台给的token存起来
              localStorage.setItem('token', res.data.data.token)
              // 跳转到home 组件
              this.$router.push('/home')
            } else {
              this.$message.error(res.data.meta.msg)
            }
          })
        }
      })
    },
    loginform () {
      this.$router.push('/home')
    }
  }
}
</script>

<style lang="less">
.login {
  width: 100%;
  height: 100%;
  background-color: #2d434c;
  overflow: hidden;
  .el-form {
    width: 400px;
    background-color: #fff;
    margin: 200px auto;
    padding: 75px 40px 15px;
    border-radius: 10px;
    position: relative;
    img {
      width: 110px;
      height: 110px;
      position: absolute;
      left: 50%;
      transform: translate(-50%);
      top: -50px;
      border-radius: 50%;
      border: 10px solid #fff;
    }

    .el-button ~ .el-button {
      margin-left: 80px;
    }
  }
}
</style>
