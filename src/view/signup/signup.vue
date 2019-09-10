<style lang="less">
@import "./signup.less";
</style>

<template>
  <div class="login">
    <div class="login-con">
      <Card icon="log-in" title="欢迎注册" :bordered="false">
        <div class="form-con">
          <signup-form @on-success-valid="handleSubmit"></signup-form>
          <!-- <p class="login-tip">输入任意用户名和密码即可</p> -->
        </div>
      </Card>
    </div>
  </div>
</template>

<script>
import SignupForm from "_c/signup-form";
import { mapActions } from "vuex";
export default {
  components: {
    SignupForm
  },
  methods: {
    ...mapActions(["handleLogin", "getUserInfo"]),
    // handleSubmit({ userName, password }) {
    //   this.handleLogin({ userName, password }).then(res => {
    //     this.getUserInfo().then(res => {
    //       this.$router.push({
    //         name: this.$config.homeName
    //       });
    //     });
    //   });

      handleSubmit({ userName, password }) {
      // this.handleLogin({ userName, password }).then(res => {
        // this.getUserInfo().then(res => {
          this.$router.push({
            name: this.$config.homeName
          });
        // });
      // });

    }
  }
};
</script>

<style>
</style>
