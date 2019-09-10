<template>
  <div>
    <div style="margin-bottom: 3px">
      <Row>
        <label v-if="confirmed" style="font-size: 20px">正在服务用户 </label>
        <!-- <label v-if="!confirmed" style="font-size: 20px" v-show="get">请接单</label> -->
        <Button v-if="confirmed" style="float: right" v-on:click="logout" type="warning">结束服务</Button>
      </Row>
    </div>

    <div v-if="!confirmed">
      <Row :gutter="20" style="margin-top: 10px;">
        <i-col :md="24" :lg="8" style="margin-bottom: 20px;">
          <Card style="width:320px">
            <div style="text-align:center">
              <!-- <img src="../../images/logo.png"> -->
              <Button type="primary" shape="circle" @click="checkJob">检查请求</Button>
            </div>
          </Card>
        </i-col>
        <i-col :md="24" :lg="8" style="margin-bottom: 20px;">
          <Card style="width:320px" v-show="get">
            <div style="text-align:center">
              <!-- <img src="../../images/logo.png"> -->
              <Button type="primary" shape="circle" @click="confirm">接单</Button>
            </div>
          </Card>
          <Card style="width:320px" v-show="!get">
            <div style="text-align:center">
              <!-- <img src="../../images/logo.png"> -->
              <Button type="primary" shape="circle">当前没有订单</Button>
            </div>
          </Card>
        </i-col>
        <!-- <i-col :md="24" :lg="24" style="margin-bottom: 20px;">
        <Card shadow>
          <chart-bar style="height: 300px;" :value="barData" text="每周用户活跃量"/>
        </Card>
        </i-col>-->
      </Row>
      <!-- <Row>
        <Col span="10">
          <Button v-on:click="checkJob">检查请求</Button>
        </Col>
      </Row>
      <Row>
        <Col span="20">
          <Button slot="append" @click="confirm">接单</Button>
        </Col>
      </Row>-->
      <!-- <label style="font-size: 20px">输入目标用户名:</label> -->
      <!-- <Input placeholder="target user name" v-model="toUser" style="margin: 2px; width: 250px;"> -->

      <!-- </Input> -->
    </div>
    <div
      v-if="confirmed"
      style=" border: Gainsboro 1px solid;background: WhiteSmoke; padding: 4px;"
    >
      <div>
        <chatview ref="chat_view" style=" margin: 2px;" :height="height"></chatview>
      </div>
<!--      <a v-if="confirmed" href="#" class="fileBox">-->
<!--        <img src="../../assets/icons/pic.png" style="width: 30px" />-->
<!--        <input-->
<!--          type="file"-->
<!--          :id="id"-->
<!--          name="image"-->
<!--          class="getImgUrl_file"-->
<!--          @change="imageInput($event)"-->
<!--        />-->
<!--      </a>-->
      <Input v-model="message_content" style="margin: 2px;width: 95%;float: right">
        <Button @click="sendSingleTo(toUser)" slot="append">发送</Button>
      </Input>
    </div>
    <!--    <input id="file_input" type="file">-->
    <!--    <input type="file" @change="getFile($event)">-->
  </div>
</template>

<script>
import Chatview from "../../components/chat/chatview";
import axios from "axios";

export default {
  name: "jmessage",
  components: { Chatview },
  data() {
    return {
      username: "未登录",
      message_content: "",
      toUser: "",
      confirmed: false,
      height: document.body.clientHeight / 1.5,
      id: "inputf",
      get: false
    };
  },
  mounted: function() {
    this.initJmessage();
    if(sessionStorage.getItem("confirmed"))
    var test = sessionStorage.getItem("confirmed")
      if(test === "false")
          this.confirmed = false
      else
          this.confirmed = true

      // console.log(typeof (confirm))
      console.log("confirm")
      console.log(this.confirmed)
  },
  methods: {
    checkJob() {
      axios
        .get("http://202.120.40.8:30454/translate/translator/checkjob")
        .then(response => {
          console.log(response);
          this.get = response.data;
          console.log(this.get);
        });
    },
    initJmessage: function() {
      let appkey = "6a8022404564c09c1622da33";
      let time = new Date().getTime().toString();
      let random_str = "022cd9fd995849b58b3ef0e943421ed9";
      let secret = "2361ed70808bbea38749f983";
      let signature = hex_md5(
        "appkey=" +
          appkey +
          "&timestamp=" +
          time +
          "&random_str=" +
          random_str +
          "&key=" +
          secret
      );
      window.JIM = new JMessage();
      JIM.init({
        appkey: appkey,
        random_str: random_str,
        signature: signature,
        timestamp: time,
        flag: 0
      })
        .onSuccess(function(data) {
          console.log(data);
        })
        .onFail(function(data) {
          console.log(data);
        });
      JIM.onSyncConversation(function(data) {
        console.log(data);
      });
    },
    // register: function () {
    //     JIM.register({
    //         'username': 'test3',
    //         'password': 'pass3',
    //         'is_md5': false,
    //         // 'extras' : {'key1':'val1','key2':'val2'},
    //         'address': '深圳'
    //     }).onSuccess(function (data) {
    //         console.log(data)
    //         //data.code 返回码
    //         //data.message 描述
    //     }).onFail(function (data) {
    //         console.log(data)
    //         // 同上
    //     })
    // },
    login: function() {
      let username = sessionStorage.getItem("username");
      let pwd = sessionStorage.getItem("password");
      let that = this;
      console.log(username);
      console.log(pwd);
      JIM.login({
        username: username,
        password: pwd,
        is_md5: true
      })
        .onSuccess(function(data) {
          console.log(data);
          that.username = data.username;
          that.bindReceive();
          let masterid = sessionStorage.getItem("masterid")
          axios
            .get("http://202.120.40.8:30454/translate/translator/getjob?translatorid=" + masterid)
            .then(response => {
              console.log(response.data);
              let to = response.data.userId.toString()
              that.toUser = to + to + to + to
                console.log("that.toUser")
                console.log(that.toUser)
            });
        })
        .onFail(function(data) {
          console.log("登陆失败");
          console.log(data.code);
          if (data.code === 880103) {
            that.signup();
          }
        });
    },
    signup: function() {
      let username =sessionStorage.getItem("username");
      let pwd = sessionStorage.getItem("password");
      let that = this;
      console.log(username);
      console.log(pwd);
      JIM.register({
        username: username,
        password: pwd,
        is_md5: true,
        extras: { key1: "val1", key2: "val2" },
        address: "上海"
      })
        .onSuccess(function(data) {
          //data.code 返回码
          //data.message 描述
          console.log("注册成功");
          that.login();
        })
        .onFail(function(data) {
          // 同上
          console.log("注册失败");
        });
    },
    getConversation: function() {
      JIM.getConversation()
        .onSuccess(function(data) {
          console.log(data);
        })
        .onFail(function(data) {});
    },
    sendSingleTo: function() {
      console.log(this.username + " " + this.toUser);
      console.log(this.message_content);
      let that = this;
      JIM.sendSingleMsg({
        target_username: that.toUser,
        target_nickname: "",
        content: that.message_content,
        appkey: "6a8022404564c09c1622da33",
        extras: ""
      })
        .onSuccess(function(data, msg) {
          console.log(data);
          console.log(msg);
          that.$refs.chat_view.addMessage(msg.content.msg_body.text, 2, 1);
          that.message_content = "";
        })
        .onFail(function(data) {
          console.log(data);
        });
    },
    bindReceive: function() {
      let that = this;
      JIM.onMsgReceive(function(data) {
        if (data.messages[0].content.msg_type === "image") {
          JIM.getResource({
            media_id: data.messages[0].content.msg_body.media_id
          })
            .onSuccess(function(data) {
              console.log(data);
              that.$refs.chat_view.addMessage(data.url, 1, 2);
              //data.code 返回码
              //data.message 描述
              //data.url 资源临时访问路径，具体超时时间expire time会包含在url中
            })
            .onFail(function(data) {
              //data.code 返回码
              //data.message 描述
            });
        } else if (data.messages[0].content.msg_type === "voice") {
          let duration = data.messages[0].content.msg_body.duration;
          JIM.getResource({
            media_id: data.messages[0].content.msg_body.media_id
          })
            .onSuccess(function(data) {
              console.log(data);
              that.$refs.chat_view.addMessage(data.url, 1, 3, duration);
              //data.code 返回码
              //data.message 描述
              //data.url 资源临时访问路径，具体超时时间expire time会包含在url中
            })
            .onFail(function(data) {
              //data.code 返回码
              //data.message 描述
            });
        } else {
          that.$refs.chat_view.addMessage(
            data.messages[0].content.msg_body.text,
            1,
            1
          );
        }
        console.log(data);
      });
    },
    logout: function() {
      console.log("logout");
      JIM.loginOut();
      this.username = "未登录";
      this.initJmessage();
      sessionStorage.setItem("confirmed", "false")
      this.confirmed = false;
      console.log(this.confirmed)
      this.checkJob()
    },
    confirm: function() {
      this.login();
      console.log(this.toUser);
      sessionStorage.setItem("confirmed", "true")
      this.confirmed = true;
      console.log(this.confirmed)
    },
    imageInput: function(event) {
      let that = this;
      let fileFormData = new FormData();
      let file = document.getElementById(this.id).files[0];
      console.log(file);
      fileFormData.append(file.name, file);
      JIM.sendSinglePic({
        target_username: that.toUser,
        target_nickname: "",
        image: fileFormData,
        appkey: "6a8022404564c09c1622da33",
        extras: {}
      })
        .onSuccess(function(data, msg) {
          console.log(data);
          JIM.getResource({
            media_id: msg.content.msg_body.media_id
          })
            .onSuccess(function(data) {
              console.log(data);
              that.$refs.chat_view.addMessage(data.url, 2, 2);
              //data.code 返回码
              //data.message 描述
              //data.url 资源临时访问路径，具体超时时间expire time会包含在url中
            })
            .onFail(function(data) {
              //data.code 返回码
              //data.message 描述
            });
          //data.code 返回码
          //data.message 描述
          //data.msg_id 发送成功后的消息id
          //data.ctime_ms 消息生成时间,毫秒
          //data.appkey 用户所属 appkey
          //data.target_username 用户名
          //msg.content 发送成功消息体
        })
        .onFail(function(data) {
          //同发送单聊文本
        });
    }
  }
};
</script>

<style scoped>
.fileBox {
  display: inline-block;
  width: 50px;
  height: 30px;
  background: WhiteSmoke;
  position: relative;
  overflow: hidden;
  cursor: pointer;
  margin: 3px;

  text-align: center;
  line-height: 30px;
  text-decoration: none;
  color: black;
  font-size: 14px;
}

.fileBox:hover {
  background: Gainsboro;
}

.fileBox input {
  position: absolute;
  left: 0;
  top: 0;
  opacity: 0;
  font-size: 50px;
  filter: alpha(opacity=0);
}
</style>
