<template>
  <div class="vue-calendar-panel-body">
    <div class="weeks">
      <span class="week" v-for="dayIndex in weekDays">
        {{ (dayIndex - 1) | localeWeekDay(firstDay, locale) }}
      </span>
    </div>
    <div class="dates" ref="dates">
      <div class="dates-bg">
        <div class="week-row" v-for="week in currentDates">
          <div class="day-cell"
               v-for="day in week"
               @click="updateDay(day)"
               :class="{'today' : day.isToday && showOnlyMonth(day) ,'selected': isSelected(day), 'not-cur-month' : !day.isCurMonth}">
            <p class="day-number" v-if="showOnlyMonth(day)">{{ day.monthDay }}</p>
            <!-- <span class="edit" v-if="isSelected(day)">{{editText}}</span> -->
          </div>
        </div>
      </div>
    </div>
    <popup v-if="show" 
           :title="title" 
           @cancel="show=false"
           @confirm="confirm">
      <div>
        <span>备注：</span>
        <input type="text" v-model="remark">
      </div>
    </popup>
  </div>
</template>

<script>
import moment from 'moment'
import Popup from './Popup.vue'

export default {
  components: {
    Popup
  },
  data () {
    return {
      weekDays: 7,
      show: false,
      remark: null,
      selectedDay: null
    }
  },
  props: {
    firstDay: {
      type: Number,
      default: 0
    },
    locale : {
      type : String,
      default : 'en'
    },
    title: {
      type : String,
      default : '备注'
    },
    currentDates: {
      type : Array,
      default : null
    },
    onlyMonth: {
      type : Boolean,
      default : true
    },
    selectedDays: {
      type : Array,
      default : null
    },
    editText: {
      type : String,
      default : '操作'
    },
    needRemark: {
      type : Boolean,
      default : true
    }
  },
  methods: {
    updateDay (day) {
      if (this.showOnlyMonth(day)) {
        if (this.needRemark) {
          this.show = true
          this.selectedDay = day
        } else {
          this.$emit("changeDay", day)
        }
      }
    },
    confirm () {
      this.$emit("changeDay", this.selectedDay)
      this.show = false
    },
    showOnlyMonth (day) {
      return (this.onlyMonth && day.isCurMonth) || !this.onlyMonth
    },
    isSelected (day) {
      return this.showOnlyMonth(day) && this.selectedDays && this.selectedDays.indexOf(day.day) > -1
    }
  },
  filters: {
    localeWeekDay (weekday, firstDay, locale) {
      firstDay = parseInt(firstDay);
      const localMoment = moment().locale(locale);
      return localMoment.localeData().weekdaysShort()[(weekday + firstDay) % 7];
    }
  }
}
</script>

<style>
.vue-calendar-panel-body .weeks {
  border: 1px solid #e7e5e1;
  border-right: none;
  display: -webkit-flex; /* Safari */
  display: flex;
  clear: both;
}
.vue-calendar-panel-body .weeks .week {
  border-right: 1px solid #e7e5e1;
  padding-top: 5px;
  padding-bottom: 5px;
  flex: 1;
  cursor: pointer;
}
.vue-calendar-panel-body .dates .week-row {
  border-left: 1px solid #e0e0e0;
  display: flex;
}
.vue-calendar-panel-body .dates .week-row .day-cell {
  padding: 1px;
  cursor: pointer;
  flex: 1;
  border-right: 1px solid #e0e0e0;
  border-bottom: 1px solid #e0e0e0;
  position: relative;
}
.vue-calendar-panel-body .dates .week-row .day-cell .edit {
  position: absolute;
  color: #333;
  bottom: 6px;
  left: 54px;
  font-size: 14px;
}
.vue-calendar-panel-body .dates .week-row .day-cell.today {
  background-color: #e1e1e1;
}
.vue-calendar-panel-body .dates .week-row .day-cell.selected {
  background-color: #8eabe6;
  color: #fff;
}
.vue-calendar-panel-body .dates .week-row .day-cell .day-number {
  line-height: 3;
}
.edit-panel {

}
</style>