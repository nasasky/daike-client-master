<template>
<div class="login-container">
    <img src="../../assets/daike.png" alt="" class="logo">

    <van-cell-group>
      <van-field v-model="username" placeholder="请输入账号" label-align="left" label="账号" clearable required></van-field>
      <van-field v-model="password" placeholder="请输入密码" label-align="left" label="密码" clearable required type="password"></van-field>
      <van-field v-show="!isLogin" v-model="repassword" placeholder="请再次输入密码" label-align="left" label="重复密码" clearable required type="password"></van-field>
    </van-cell-group>

    <van-row class="box">
      <van-button size="small" @click="handleRegister">
        {{ isLogin ? '注册' : '已有账号'}}
      </van-button>
      <van-button type="primary" size="small" class="login-btn" @click="handleLogin">
        {{isLogin ? '登录' : '注册并且登录'}}
      </van-button>
    </van-row>
</div>
</template>
<script>
export default {
  data() {
    return {
     username: '',
      password: '',
      repassword: '',
      isLogin: true
    }
  },
  methods: {
    handleRegister() {
      this.isLogin = !this.isLogin
    },
    showLoginTip() {
      const toast = this.$toast.loading({
        duration: 0,
        forbidClick: false,
        loadingType: 'spinner',
        message: '登录中......'
      })
    },
    login() {
      this.$http.login({
        username: this.username,
        password: this.password
      }).then(response => {
        console.log('登录成功返回', response)
        console.log('返回code',response.code)
        if(response.code == 1) {

            this.$router.push({
                path: '/home'
            })
          // this.$store.dispatch('setUser', )
        }
      }).catch(err => {
        console.error('登录发生错误', err)
      })
    },
    handleLogin() {
      if(!this.username || !this.password) {
        this.$toast.fail('用户名或是密码不能为空')
        return
      }

      if(this.isLogin) {
        this.showLoginTip()
        this.login()
      } else {
        console.log("注册用户")
        if(this.password != this.repassword) {
          this.$toast.fail('两次输入密码不一致')
          return
        }

        this.$http.register({
            username: this.username,
          password: this.password
        }).then(res => {
          console.log('注册成功返回', res.data)
          this.$toast.clear()
          // this.$store.dispath('setUser', res.data)
          this.$router.push({
            path: '/home'
          })
        }).catch(err => {
          this.$toast.fail(err)
        })
      }
    }

  }
}
</script>
<style lang="scss" scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  .logo {
    width: 300px;
    margin: 100px 0 30px;
  }
  .box {
    margin-top: 30px;
  }
  .login-btn {
    margin-left: 20px;
  }
}
</style>
