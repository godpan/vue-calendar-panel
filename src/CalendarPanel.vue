<template>
  <div id="app" class="vue-calendar-panel">
    <panel-header 
      :locale="locale" 
      :currentMonth="curMonth"
      :stylePattern="stylePattern"
      :monthFormat="monthFormat"
      @changeMonth="changeMonth"></panel-header>
    <panel-body 
      :locale="locale"
      :currentDates="currentDates"
      :selectedDays="selectedDays"
      :needRemark="needRemark"
      :onlyMonth="onlyMonth"
      :title="title"
      :leftArrow="leftArrow"
      @changeDay="changeDay"></panel-body>
  </div>
</template>

<script>
import PanelHeader from './components/PanelHeader.vue'
import PanelBody from './components/PanelBody.vue'
import moment from 'moment'
import DateUtil from './util/DateUtil'

export default {
  components: {
    PanelHeader,
    PanelBody
  },
  props: {
    locale: {
      type: String,
      default: 'zh-cn'
    },
    defaultDays: {
      type : Array,
      default : null
    },
    mode: {
      type : String,
      default : 'single'
    },
    title: {
      type : String,
      default : '备注'
    },
    onlyMonth: {
      type : Boolean,
      default : true
    },
    needRemark: {
      type : Boolean,
      default : false
    },
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
      default: function () {
        return moment().startOf('month')
      }
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
      curMonth: null,
      selectedDays: null,
      modeSingle: 'single',
      modeMulti: 'multi'
    }
  },
  computed: {
    currentDates () {
      return this.getCalendar()
    }
  },
  mounted () {
    this.selectedDays = this.defaultDays
    this.curMonth = this.currentMonth
  },
  methods: {
    changeMonth (val) {
      this.curMonth = val
    },
    changeDay (val) {
      if (this.mode == this.modeSingle) {
        this.selectedDays = []
        this.selectedDays.push(val.day)
      } else {
        if (this.selectedDays) {
          let index = this.selectedDays.indexOf(val.day)
          if (index > -1) {
            this.selectedDays.splice(index, 1)
          } else {
            this.selectedDays.push(val.day)
          }
        } else {
          this.selectedDays = []
          this.selectedDays.push(val.day)
        }
      }
    },
    getCalendar () {
      let monthViewStartDate = DateUtil.getMonthViewStartDate(this.curMonth, this.firstDay);
      let calendar = [];
      for(let perWeek = 0 ; perWeek < 6 ; perWeek++) {
        let week = [];
        for(let perDay = 0 ; perDay < 7 ; perDay++) {
          week.push({
            day : monthViewStartDate.format('YYYY-MM-DD'),
            monthDay : monthViewStartDate.date(),
            isToday : monthViewStartDate.isSame(moment(), 'day'),
            isCurMonth : monthViewStartDate.isSame(this.curMonth, 'month'),
            weekDay : perDay,
            date : moment(monthViewStartDate)
          });
          monthViewStartDate.add(1, 'day');
        }
        if ((perWeek != 5) || (perWeek == 5 && week[0] >= 30)) {
          calendar.push(week);
        }
      }
      return calendar
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.vue-calendar-panel {
  width: 638px;
  margin: 0 auto;
}
</style>
