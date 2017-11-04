<template>
  <div class="vue-calendar-panel-header">
    <div class="header-center" v-if="stylePattern == centerPattern">
      <span class="prev-month" @click.stop="goPrev">{{leftArrow}}</span>
      <span class="current-month">{{currentMonthText}}</span>
      <span class="next-month" @click.stop="goNext">{{rightArrow}}</span>
    </div>
    <div class="header-left" v-if="stylePattern == leftPattern">
      <span class="current-month">{{currentMonthText}}</span>
      <span class="right-handle">
        <span class="prev-month" @click.stop="goPrev">{{leftArrow}}</span>
        <span class="current-day" @click.stop="goToday">今天</span>
        <span class="next-month" @click.stop="goNext">{{rightArrow}}</span>
      </span>
    </div>
  </div>
</template>

<script>
import moment from 'moment'

export default {
  props: {
    leftArrow: {
      type: String,
      default: "<"
    },
    rightArrow: {
      type: String,
      default: ">"
    },
    currentMonth: {
      type: Object,
      default: null
    },
    locale: {
      type: String,
      default: 'en'
    },
    monthFormat: {
      type: String,
      default: 'YYYY年MM月'
    },
    stylePattern: {
      type: String,
      default: 'right'
    }
  },
  data () {
    return {
      centerPattern: 'center',
      leftPattern: 'right'
    }
  },
  computed: {
    currentMonthText () {
      if (!this.currentMonth) {
        return null;
      } else {
        return this.currentMonth.locale(this.locale).format(this.monthFormat);
      }
    }
  },
  methods : {
    goPrev () {
      var newMonth = moment(this.currentMonth).subtract(1, 'months').startOf('month');
      this.$emit('changeMonth', newMonth);
    },
    goNext () {
      var newMonth = moment(this.currentMonth).add(1, 'months').startOf('month');
      this.$emit('changeMonth', newMonth);
    },
    goToday () {
      var newMonth = moment().startOf('month');
      this.$emit('changeMonth', newMonth);
    }
  }
}
</script>

<style>
.vue-calendar-panel-header {
  margin-bottom: 20px;
  height: 30px;
  line-height: 30px;
}
.vue-calendar-panel-header .prev-month,
.vue-calendar-panel-header .next-month {
  cursor: pointer;
  border: 1px solid #e7e5e1;
  padding: 0px 4px;
  border-radius: 3px;
}
.vue-calendar-panel-header .header-left .current-month {
  float: left;
}
.vue-calendar-panel-header .header-left .current-month {
  float: left;
}
.vue-calendar-panel-header .header-left .right-handle {
  float: right;
  font-size: 12px;
}
.vue-calendar-panel-header .header-left .current-day {
  cursor: pointer;
  border: 1px solid #e7e5e1;
  padding: 0px 4px;
  border-radius: 3px;
}
</style>
