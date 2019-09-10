<template>
  <Form ref="loginForm" :model="form" :rules="rules" @keydown.enter.native="handleSubmit">
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
      <Button @click="handleSignin" type="primary" long>注册</Button>
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
        return [{ required: true, message: "账号不能为空", trigger: "blur" },
            { type: 'string', min: 4, message: '用户名长度不能小于4位', trigger: 'blur' }];
      }
    },
    passwordRules: {
      type: Array,
      default: () => {
        return [{ required: true, message: "密码不能为空", trigger: "blur" },
            { type: 'string', min: 6, message: '密码长度不能小于6位', trigger: 'blur' }];
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
    handleSignin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          axios
            .get(
              "http://202.120.40.8:30454/translate/translator/registertranslator?username=" +
                this.form.userName +
                "&password=" +
                this.form.password
              // "http://localhost:7000/api/translate/translator/signin?username=" +
              //   this.form.userName +
              //   "&password=" +
              //   this.form.password
            )
            .then(response => {
              console.log(response);
              if (response.data === "success") {
                console.log("注册成功");
                  this.$Message.success('注册成功');
                this.$router.push({
                  name: this.$config.homeName
                });
              }
              else if(response.data === "username already exist"){
                  console.log("用户名已经存在")
                  this.$Message.error('用户名已存在');
              }
            });
        }
      });
    }
  }
};
</script>
