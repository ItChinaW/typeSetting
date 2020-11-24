<template>
  <div class="container">
    <div id="logo">
      <img src="@/static/plogo.png">
      <p style="font-size: 12px">--国内首款安全、无广告的排版工具</p>
      <div class="context">
        <textarea v-model="textValue" id="foo"></textarea>
        <p>
          <button @click="clearHtml()">清除html</button>
          <button @click="strictFormat()">精密排版</button>
          <button @click="freeFormat()">一键排版</button>
          <button @click="replaceFormat()">替换标点</button>
          <button @click="SimToComFormat()">{{ textSimToCom }}</button>
          <button @click="totallCount()">统计数字</button>
          <button class="btn" @click="copy()">复制内容</button>
          <button @click="clear()">清空内容</button>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import {returnS, returnT} from '../utils/index'
import Clipboard from 'clipboard'

export default {
  data() {
    return {
      textValue: '',
      tag: false,
      textSimToCom: "简->繁"
    }
  },
  methods: {
    // 清除html
    clearHtml() {
      const str = this.textValue;
      this.textValue = str.replace(/<[^>].*?>/g, "")
    },
    // 精密排版
    strictFormat() {
      this.clearHtml()
      let str = ""
      str = this.textValue.replace(/[\r\n]+/g, "\n").replace(/[\t]/mg, "")
      str = str.replace(/[ ]/g, "")
      str = str.replace(/^\s*/mg, "    ")
      this.textValue = str
    },
    // 一键排版
    freeFormat() {
      let str = ""
      str = this.textValue.replace(/[\n]+/mg, "\n\n").replace(/[\n]$/g)
      this.textValue = str
    },
    // 替换标点
    replaceFormat() {
      this.textValue = this.textValue.replace(/\./mg, '。')
      this.textValue = this.textValue.replace(/\,/mg, '，')
      this.textValue = this.textValue.replace(/\?/mg, '？')
    },
    // 简->繁 繁->简
    SimToComFormat() {
      let str = this.textValue
      let s = returnS()
      let t = returnT()
      let k = ""
      if (this.tag == false) {
        this.tag = true
        this.textSimToCom = "繁->简"
        for (let i = 0; i < str.length; i++) {
          k += s.indexOf(str.charAt(i)) == -1 ? str[i] : t.charAt(s.indexOf(str.charAt(i)))
        }
        this.textValue = k
      } else {
        this.tag = false
        this.textSimToCom = "简->繁"
        for (let i = 0; i < str.length; i++) {
          k += t.indexOf(str.charAt(i)) == -1 ? str[i] : s.charAt(t.indexOf(str.charAt(i)))
        }
        this.textValue = k
      }

    },
    // 统计字数
    totallCount() {
      alert("目前长度为：" + this.textValue.length + "个文字")
    },
    copy() {
      let txt = this.textValue;
      let clipboard = new Clipboard('.btn', {
        text: function () {
          return txt
        }
      });
      clipboard.on('success', e => {
        console.log(this, '复制成功!', 'success');
        // 释放内存
        clipboard.destroy()
      })
      clipboard.on('error', e => {
        // 不支持复制
        console.log(this, '该浏览器不支持自动复制!', 'warning');
        // 释放内存
        clipboard.destroy()
      })
    },
    clear(){
      document.getElementById("foo").focus()
      this.textValue = ""
    },
  }
}
</script>

<style lang="less">
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;

  .context {
    width: 950px;
    margin: 20px auto;
    border: 1px solid lawngreen;

    textarea {
      margin: 15px 0px 10px;
      width: 915px;
      height: 400px;
      border: 1px solid lawngreen;
      background-color: #f3fff1;
    }

    p {
      width: 80%;
      margin: 0 auto;
      display: flex;
      justify-content: space-between;

      button {
        padding: 7px;
        margin-bottom: 10px;
      }
    }
  }
}

</style>
