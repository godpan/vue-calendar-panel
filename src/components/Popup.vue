<template>
  <div class="ui-popup-overlay">
    <div class="ui-popup-box" :class="[size]" ref="popBox" :style="boxStyle">
      <div class="pop-hd" v-if="withHeader">
        <h3>{{title}}</h3>
      </div>
      <div class="pop-bd">
        <slot></slot>
      </div>
      <slot name="footer"></slot>
      <div class="pop-ft" v-if="withFooter">
        <div class="pop-action">
          <btn v-if="buttons.yes && buttons.yes.show"
               styles="flat" color="gold" @click.native="$emit('confirm')">{{buttons.yes.text}}</btn>
          <btn v-if="buttons.no && buttons.no.show"
               styles="flat" color="grey" @click.native="$emit('cancel')">{{buttons.no.text}}</btn>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Btn from './Btn.vue'

export default {
  components: {
    Btn
  },
  data () {
    return {
      boxStyle: {
        marginTop: '0px',
        marginLeft: '0px'
      }
    }
  },
  watch: {
    'show' () {
      this.resize()
    }
  },
  methods: {
    resize () {
      this.$nextTick(() => {
        const rect = this.$refs.popBox.getBoundingClientRect()
        this.boxStyle = {
          marginLeft: (-rect.width / 2) + 'px',
          marginTop: (-rect.height / 2) + 'px'
        }
        if (this.width) {
          this.boxStyle.width = this.width + 'px'
        }
      })
    }
  },
  props: {
    title: {
      type: String,
      required: true
    },
    withHeader: {
      type: Boolean,
      default: true
    },
    withFooter: {
      type: Boolean,
      default: true
    },
    show: {
      type: Boolean
    },
    buttons: {
      type: Object,
      default () {
        return {
          yes: {show: true, text: '确定'},
          no: {show: true, text: '取消'}
        }
      }
    },
    size: {
      type: String,
      default: 'large'
    },
    width: {
      type: Number,
      default: null
    }
  },
  mounted () {
    this.resize()
  }
}
</script>

<style>
.popup-enter-active, .popup-leave-active {
  transition:opacity .3s ease;
}
.popup-enter,
.popup-leave-to {
  opacity: 0;
}
.ui-popup-box {
  position: absolute;
  top: 50%;
  left: 50%;
  background: #ffffff;
  width: auto;
  pointer-events: auto;
  -webkit-transition: all .3s;
  -moz-transition: margin .3s;
  -o-transition: margin .3s;
  transition: all .3s;
}
.ui-popup-overlay {
  top:0;
  left:0;
  position: fixed;
  height: 100%;
  width: 100%;
  background: rgba(128, 128, 128, 0.75);
  z-index: 888;
}
.ui-popup-box .pop-close {
  display: block;
  position: absolute;
  right: 0;
  top: 0;
  height: 40px;
  width: 51px;
}
.ui-popup-box .pop-hd h3 {
  color: var(--color-gray3);
  font-size: 14px;
  padding-left: 20px;
  float: left;
  display: inline-block;
  margin-top: 0px;
}
.ui-popup-box.large {
  width: 650px;
}
.ui-popup-box.middle {
  width: 450px;
}
.ui-popup-box .pop-bd {
  padding: 30px 40px;
  font-size: 14px;
  overflow: hidden;
}
.ui-popup-box .pop-hd {
  background: #f2f2f2;
  height: 40px;
  line-height: 40px;
}
.ui-popup-box .pop-ft {
  height: 49px;
  background: #fcfbfb;
  position: relative;
}
.ui-popup-box .pop-ft .pop-action {
  position: absolute;
  right: 10px;
  top: 10px;
}
.ui-popup-box .pop-hd .pop-close em {
  width: 50px;
  height: 40px;
  position: absolute;
  right: 0;
  top: 0;
  display: block;
}
.ui-popup-box .pop-hd .pop-close:hover em {
  background-position: 15px -128px;
}
</style>
