<template>
  <div style="margin: 2px; float: top; width: 100%;">
    <!--    <label>chat view</label>-->
    <!--    <audio src="" id="eventAudio"></audio>-->
    <div ref="chat_box" :style="{background: 'WhiteSmoke', overflow: 'auto', height: height+'px'}">
      <Row v-for="(item, i) in testList1" :key="i" style="width: 100%; margin-top: 5px">
        <Col span="12" style="">
          <Avatar size="large" v-if="item.mode === 1" style="float: left; margin: 5px"
                  src="https://i.loli.net/2017/08/21/599a521472424.jpg"/>
          <Card v-if="item.mode === 1 && item.type === 1"
                style="float: left; margin: 5px; max-width: 80%; word-wrap:break-word">
            {{item.text}}
          </Card>
          <Card v-if="item.mode === 1 && item.type === 2"
                style="float: left; margin: 5px; max-width: 80%; word-wrap:break-word">
            <img style="max-width: 100%" :src="item.text">
          </Card>
          <Card v-if="item.mode === 1 && item.type === 3"
                style="float: left; margin: 5px; max-width: 80%; word-wrap:break-word; border-radius: 10px">
            <audio :src="item.text" controls="controls"></audio>
            <!--            <div>-->
            <!--              <img :id="i.toString()" style="width: 30px" clickMusic="true" :a_src="item.text"-->
            <!--                   src="../../assets/icons/play.png"/>-->
            <!--              <label style="margin-top: 5px; margin-left: 5px;  float: right; display: block">{{item.extra}}</label>-->
            <!--            </div>-->
          </Card>
          <label v-if="item.mode === 2" style="width: 10px">&#12288;</label>
        </Col>
        <Col span="12" style="">
          <Avatar size="large" v-if="item.mode === 2" style="float: right; margin: 5px  "
                  src="https://i.loli.net/2017/08/21/599a521472424.jpg"/>
          <Card v-if="item.mode === 2 && item.type === 1"
                style="float: right; margin: 5px; max-width: 80%; word-wrap:break-word">
            {{item.text}}
          </Card>
          <Card v-if="item.mode === 2 && item.type === 2"
                style="float: right; margin: 5px; max-width: 80%; word-wrap:break-word">
            <img style="max-width: 100%" :src="item.text">
          </Card>
        </Col>
      </Row>
    </div>
  </div>
</template>

<script>
    export default {
        name: 'chatview',
        props: { 'height': Number },
        data () {
            return {
                testList1: [],
                src: ''
            }
        },
        methods: {
            addMessage: function (messageBody, mode, type, extra) {
                let div = this.$refs.chat_box
                this.testList1.push({
                    'text': messageBody,
                    'mode': mode,
                    'type': type,
                    'extra': extra || {}
                })
                setTimeout(() => {
                    // console.log(`点击了添加按钮，更新时间：${time},此时的div.scrollHeight是：${div.scrollHeight}`)
                    div.scrollTop = div.scrollHeight
                }, 0)
            },
            // play: function (i) {
            //     this.$refs.a1.play()
            //     // let audio = document.getElementById(i.toString())
            //     // audio.play()
            // }

        },
        // mounted: function () {
        //     // let that = this;
        //     document.body.addEventListener('click', function (e) {
        //         let event = e || window.event
        //         let target = event.target || event.srcElement
        //         let clickMusic = target.getAttribute('clickMusic')
        //         let src = target.getAttribute('a_src')
        //         let id = target.getAttribute('id')
        //         if (clickMusic === 'true') {
        //             let img = document.getElementById(id)
        //             let buttonAudio = document.getElementById('eventAudio')
        //             buttonAudio.setAttribute('src', src)
        //             buttonAudio.play()
        //             if (buttonAudio.paused) {
        //                 // 开始播放当前点击的音频
        //                 buttonAudio.play()
        //                 // img.setAttribute('src', '../../assets/icons/pause.png')
        //                 // $('#audioPlayer').attr('src', './image/pause.png');
        //                 // $('#progressBarBg').css('cursor', 'pointer');
        //             } else {
        //                 buttonAudio.pause()
        //                 // img.setAttribute('src', '../../assets/icons/play.png')
        //                 // $('#audioPlayer').attr('src', './image/play.png');
        //                 // $('#progressBarBg').css('cursor', 'default');
        //             }
        //         } else {
        //             return false
        //         }
        //     })
        // }
    }
</script>

<style scoped>
  ::-webkit-scrollbar {
    width: 0 !important;
  }

  ::-webkit-scrollbar {
    width: 0 !important;
    height: 0;
  }
</style>
