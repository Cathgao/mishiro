<template>
<div v-show="show" class="modal">
  <transition name="scale" @after-leave="afterLeave">
    <div v-show="visible" :style="{ width: modalWidth }">
      <div class="modal-header">
        <title-dot v-once></title-dot>
        <h4 class="modal-title">{{title}}</h4>
      </div>
      <div class="modal-body" :style="{ maxHeight: bodyMaxHeight }" v-html="body"></div>
      <div class="modal-footer">
        <button type="button" class="cgss-btn cgss-btn-default" @click="close">{{$t("home.close")}}</button>
      </div>
    </div>
  </transition>
</div>
</template>

<script>
import modalMixin from '../../js/renderer/modalMixin.js'
export default {
  mixins: [modalMixin],
  data () {
    return {
      title: '',
      body: ''
    }
  },
  methods: {
    afterLeave () {
      this.show = false
      this.title = ''
      this.body = ''
      this.modalWidth = '600px'
    }
  },
  mounted () {
    this.$nextTick(() => {
      this.event.$on('alert', (title, body, width) => {
        if (width) this.modalWidth = width + 'px'
        this.title = title
        this.body = body
        this.show = true
        this.visible = true
      })
    })
  }
}
</script>

<style>
.modal {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 10000;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.modal > div {
  border: 2px solid rgba(0, 0, 0, 1);
  border-radius: 10px;
  font-family: "CGSS-B";
  font-size: 20px;
}
.modal-header {
  padding: 18px;
  border-bottom: 1px solid #000;
  height: 64px;
  background: linear-gradient(180deg, #f0f0f0, #d0d0d0);
  border-radius: 10px 10px 0 0;
}
.modal-title {
  margin: 0;
  line-height: 27px;
  font-size: 20px;
  height: 27px;
  display: inline-block;
  position: relative;
  bottom: 5px;
}
.modal-body {
  position: relative;
  padding: 15px;
  background-color: rgba(0, 0, 0, 0.87);
  color: #fff;
  overflow: auto;
}
.modal-footer {
  padding: 15px;
  border-top: 1px solid #000;
  background: linear-gradient(180deg, #f0f0f0, #d0d0d0);
  height: 103px;
  border-radius: 0 0 10px 10px;
  /* text-align: center; */
  display: flex;
  justify-content: center;
  align-content: center;
}
</style>
