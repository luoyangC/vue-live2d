<template>
  <div class="live2d">
    <div class="live2d-card">
      <div class="live2d-card__message">{{ tipText }}</div>
      <canvas id="live2d-main" class="live2d-card__canvas" width="200" height="200" />
      <div class="live2d-card__input">
        <div class="input-name">
          <input name="name" type="text" class="input-name__inner" autocomplete="off" placeholder="你的名字">
        </div>
        <div class="input-talk">
          <input name="talk" type="text" class="input-talk__inner" autocomplete="off" placeholder="要和我聊什么呀？">
          <button type="button" class="input-talk__btn">发送</button>
        </div>
      </div>
      <div class="live2d-card__action">
        <div class="live2d-card__icon action-icon--info" />
        <div class="live2d-card__icon action-icon--talk" />
        <div class="live2d-card__icon action-icon--youdu" />
        <div class="live2d-card__icon action-icon--hidden" />
        <input value="0" type="hidden">
        <input value="douqilai,l2d_caihong" type="hidden">
      </div>
    </div>
    <div class="live2d-btn">看板娘</div>
  </div>
</template>

<script>
import './live2d.min.js'

export default {
  data() {
    return {
      messageTimer: null,
      mainShow: true,
      tipText: ''
    }
  },
  mounted() {
    this.loadModel()
    this.$nextTick(() => {
      this.loadEvent()
    })
  },
  methods: {
    loadModel() {
      const url = 'http://www.luoyangc.cn:22340/api/model?name=BiliBili-22'
      const callback = console.log(`Live2D 模型加载完成`)
      window.loadlive2d('live2d-main', url, callback)
    },
    loadEvent() {
      for (const event in this.tips) {
        for (const obj of this.tips[event]) {
          const selector = obj.selector
          let dom = null

          if (selector === 'document') {
            dom = document
          } else if (document.querySelector(selector)) {
            dom = document.querySelector(selector)
          }

          if (dom == null) continue
          dom.addEventListener(event, () => {
            const arr = obj.text
            const msg = arr[Math.floor(Math.random() * arr.length)]
            this.showMessage(msg, 2000)
          })
        }
      }
    },
    showMessage(msg = '', timeout = 6000) {
      if (this.messageTimer) {
        clearTimeout(this.messageTimer)
        this.messageTimer = null
      } else {
        this.tipShow = true
      }
      this.tipText = msg
      this.messageTimer = setTimeout(() => {
        this.tipShow = false
        this.messageTimer = null
      }, timeout)
    }
  }
}
</script>

<style scoped>
.live2d-card {
  user-select: none;
  position: fixed;
  left: 5px;
  bottom: 0;
  width: 250px;
  height: 280px;
  z-index: 10000;
  font-size: 0;
}
.live2d-btn {
  position: fixed;
  left: 5px;
  bottom: 5px;
  height: 24px;
  line-height: 24px;
  padding: 2px 10px;
  border: 2px solid rgba(75, 127, 199, 0.9);
  border-radius: 2px;
  background-color: rgba(74, 59, 114, 0.9);
  color: #fff;
  font-size: 12px;
  z-index: 10000;
  cursor: pointer;
}

.live2d-card__canvas {
  position: relative;
  height: 280px;
  width: 250px;
  z-index: 3;
}

.live2d-card__message {
  position: absolute;
  left: 0px;
  bottom: 280px;
  height: auto;
  width: 250px;
  margin: auto;
  padding: 7px;
  opacity: 0;
  color: #fff;
  box-sizing: border-box;
  text-align: center;
  border: 2px solid rgba(75, 127, 199, 0.9);
  border-radius: 5px;
  background-color: rgba(74, 59, 114, 0.9);
  font-size: 13px;
  font-weight: 400;
  text-overflow: ellipsis;
  text-transform: uppercase;
  overflow: hidden;
  animation-delay: 5s;
  animation-duration: 50s;
  animation-iteration-count: infinite;
  animation-name: shake;
  animation-timing-function: ease-in-out;
}

</style>
