<template>
  <Form ref="loginForm" :model="form" :rules="rules" @keydown.enter.native="login">
    <FormItem prop="userName">
      <Input v-model="form.userName" placeholder="请输入用户名">
        <span slot="prepend">
          <Icon :size="16" type="ios-person"></Icon>
        </span>
      </Input>
    </FormItem>
    <FormItem prop="password">
      <Input type="password" v-model="form.password" placeholder="请输入密码">
        <span slot="prepend">
          <Icon :size="14" type="md-lock"></Icon>
        </span>
      </Input>
    </FormItem>
    <FormItem>
      <Button @click="login" type="primary" long>登录</Button>
    </FormItem>
  </Form>
</template>
<script>
import axios from "axios";

export default {
  name: "LoginForm",
  props: {
    userNameRules: {
      type: Array,
      default: () => {
        return [{ required: true, message: "账号不能为空", trigger: "blur" }];
      }
    },
    passwordRules: {
      type: Array,
      default: () => {
        return [{ required: true, message: "密码不能为空", trigger: "blur" }];
      }
    }
  },
  data() {
    return {
      form: {
        userName: "",
        password: ""
      }
    };
  },
  computed: {
    rules() {
      return {
        userName: this.userNameRules,
        password: this.passwordRules
      };
    }
  },
  methods: {
    handleSubmit() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.$emit("on-success-valid", {
            userName: this.form.userName,
            password: this.form.password
          });
        }
      });
    },
    login() {
      console.log("点击登录")
      this.$refs.loginForm.validate(valid => {
        console.log("连接请求")
        if (valid) {
          console.log("合法")
          axios
            .get(
              "http://localhost:7000/api/translate/translator/signin?username=" +
                this.form.userName +
                "&password=" +
                this.form.password
            )
            .then(response => {
              console.log("登录")
              console.log(response);
              if (response.data != 0) {
                console.log("登录成功");
                this.$Message.success('登录成功');
                sessionStorage.setItem("masterid", response.data)
                sessionStorage.setItem("username", this.form.userName)
                sessionStorage.setItem("password", this.form.password)
                sessionStorage.setItem("confirmed", "false")
                this.$emit("on-success-valid", {
                  userName: 'admin',
                  password: 'admin'
                });
                // this.$router.push({
                //   name: this.$config.homeName
                // });
              }
              else{
                  this.$Message.error('密码错误');
              }
            })
              .catch(error => {
                  this.$Message.error('用户名不存在');
              });
        }
      });
    }
  }
};
</script>
