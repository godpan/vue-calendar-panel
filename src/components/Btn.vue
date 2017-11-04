<template>
  <button
    class="ui-btn"
    :class="[styles, color, size, status]"
    :disabled="disabled || status === STATUS.ING" 
    @click="goto"><slot></slot></button>
</template>

<script>
const STATUS = {
  ING: 'ing'
}

export default {
  methods: {
    goto () {
      if (this.href) {
        window.location.href = this.href
      } else {
        this.$emit('click')
      }
    }
  },
  props: {
    size: {
      type: String,
      default: ''
    },
    styles: {
      type: String,
      default: ''
    },
    status: {
      type: String,
      default: ''
    },
    color: {
      type: String
    },
    disabled: {
      type: Boolean,
      default: false
    },
    href: {
      type: String
    }
  },
  data () {
    return {
      STATUS
    }
  }
}
</script>

<style>
a.ui-btn,button.ui-btn {
  display: inline-block;
  margin-right: 5px;
  font-size: 14px;
  height: 33px;
  line-height: 33px;
  padding: 0 20px;
  text-align: center;
  vertical-align: middle;
  user-select: none;
  border-radius: 3px;
  box-shadow: none;
  border: none;
  outline: none;
  cursor: pointer;
}
a.ui-btn.small,button.ui-btn.small {
  font-size:12px;
  padding:0 10px;
  height:25px;
  line-height: 25px;
  border-radius: 3px;
}
a.ui-btn.gold,button.ui-btn.gold {
  color: #fff;
  background-color: #f44a56;
}
a.ui-btn.gold:active,button.ui-btn.gold:active {
  box-shadow:inset 2px 2px 3px #df680c;
}
a.ui-btn.gold:hover,button.ui-btn.gold:hover {
  background: #2e3846;
}
a.ui-btn.flat,button.ui-btn.flat {
  border-radius: 0;
}
a.ui-btn.white,button.ui-btn.white {
  color: #666;
  background: #fdfcfc;
  border: 1px solid #dadada;
  height: 30px;
  line-height: 30px;
  padding: 0 10px;
}

a.ui-btn.disabled:hover,
a.ui-btn.disabled,
button.ui-btn.disabled:hover,
button.ui-btn.disabled {
  border-radius: 0;
  color: #fff;
  background-color: #e5e5e5;
  cursor: default;
  box-shadow: none;
}
a.ui-btn.grey,
button.ui-btn.grey {
  color: #666;
  background-color: #ebebeb;
}
a.ui-btn.grey:hover,
button.ui-btn.grey:hover {
  background-color: #dedede;
}
a.ui-btn.grey,
button.ui-btn.grey:active {
  box-shadow:inset 2px 2px 3px #bbb;
}

a.ui-btn.ing,
a.ui-btn.ing:hover,
button.ui-btn.ing,
button.ui-btn.ing:hover {
  background-color: #e5e5e5;
  color: #666;
}
a.ui-btn.ing:before,
button.ui-btn.ing:before {
  content: '';
  display: inline-block;
  width: 16px;
  height: 16px;
  vertical-align: middle;
  margin-right: 3px;
}
</style>
