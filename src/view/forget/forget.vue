<style lang="less">
@import "./forget.less";
</style>

<template>
  <div class="login">
    <div class="login-con">
      <Card icon="log-in" title="欢迎登录" :bordered="false">
        <div class="form-con">
          <login-form @on-success-valid="handleSubmit"></login-form>
          <Row>
            <Col span="18">
              <Button type="text">忘记密码？</Button>
            </Col>
            <Col span="2">
              <Button type="text" @click="signUp">注册</Button>
            </Col>
          </Row>
          <!-- <p class="login-tip">输入任意用户名和密码即可</p> -->
        </div>
      </Card>
    </div>
  </div>
</template>

<script>
import LoginForm from "_c/login-form";
import { mapActions } from "vuex";
export default {
  components: {
    LoginForm
  },
  methods: {
    // ...mapActions(["handleLogin", "getUserInfo"]),
    // handleSubmit({ userName, password }) {
    //   this.handleLogin({ userName, password }).then(res => {
    //     this.getUserInfo().then(res => {
    //       this.$router.push({
    //         name: this.$config.homeName
    //       });
    //     });
    //   });

    handleSubmit({ userName, password }) {
    this.handleLogin({ userName, password }).then(res => {
      this.getUserInfo().then(res => {
        this.$router.push({
          name: this.$config.homeName
        });
      });
    });
    },
    signUp() {
      this.$router.push({
        name: this.$config.SignUpName
      });
    }
  }
};
</script>

<style>
</style>
