<template>
  <div class="__vev_calendar-wrapper">
      <div class="__vev_calendar-wrapper">
          <cal-panel
                  :events="events"
                  :calendar="calendarOptions"
                  :selectedDay='selectedDayEvents.date'
                  @cur-day-changed="handleChangeCurDay"
                  @month-changed="handleMonthChanged">
          </cal-panel>
          <!-- <cal-events
             :title="title"
             :dayEvents="selectedDayEvents"
             :locale="calendarOptions.options.locale"
             :color="calendarOptions.options.color">
             <slot :showEvents="selectedDayEvents.events"></slot>
           </cal-events>-->
      </div>
  </div>
</template>
<script>
import { isEqualDateStr} from './tools.js'

//import calEvents from './components/cal-events.vue'
import calPanel from './components/cal-panel.vue'

const inBrowser = typeof window !== 'undefined'
export default {
  name: 'vue-event-calendar-ysh',
  components: {
//    'cal-events': calEvents,
    'cal-panel': calPanel
  },
  data () {
    return {
      selectedDayEvents: {
        date: 'all',
        events: this.events || []  //default show all event
      }
    }
  },
  props: {
    title: String,
    events: {
      type: Array,
      required: true,
      default: [],
      validator (events) {
        let validate = true
        events.forEach((event, index) => {
          if (!event.date) {
            console.error('Vue-Event-Calendar-Error:' + 'Prop events Wrong at index ' + index)
            validate = false
          }
        })
        return validate
      }
    }
  },
  computed: {
    calendarOptions () {
      let dateObj = new Date()
      if (inBrowser) {
          return window.VueCalendarBarEventBus.CALENDAR_EVENTS_DATA
      } else {
        return {
          options: {
            locale: 'en', //zh
            color: ' #f29543'
          },
          params: {
              curYear: dateObj.getFullYear(),
              curMonth: dateObj.getMonth(),
              curDate: dateObj.getDate(),
              curEventsDate: 'all'
          }
        }
      }
    },
    calendarParams () {
      let dateObj = new Date()
      if (inBrowser) {
          return window.VueCalendarBarEventBus.CALENDAR_EVENTS_DATA.params
      } else {
        return {
          curYear: dateObj.getFullYear(),
          curMonth: dateObj.getMonth(),
          curDate: dateObj.getDate(),
          curEventsDate: 'all'
        }
      }
    }
  },
  created () {
    if (this.calendarParams.curEventsDate !== 'all') {
      this.handleChangeCurDay(this.calendarParams.curEventsDate)
    }
  },
  methods: {
    handleChangeCurDay (date) {
      let events = this.events.filter(function(event) {
        return isEqualDateStr(event.date, date)
      })
      if (events.length > 0) {
        this.selectedDayEvents = {
          date: date,
          events: events
        }
      }
      this.$emit('day-changed', {
        date: date,
        events: events
      })
    },
    handleMonthChanged (yearMonth) {
      this.$emit('month-changed', yearMonth)
    }
  },
  watch: {
    calendarParams () {
      if (this.calendarParams.curEventsDate !== 'all') {
        let events = this.events.filter(event => {
          return isEqualDateStr(event.date, this.calendarParams.curEventsDate)
        })
        this.selectedDayEvents = {
          date: this.calendarParams.curEventsDate,
          events
        }
      } else {
        this.selectedDayEvents = {
          date: 'all',
          events: this.events
        }
      }
    },
    events () {
      this.selectedDayEvents = {
        date: 'all',
        events: this.events || []
      }
    }
  }
}
</script>
<style lang="less">
@base-orange: #f29543;
@white: #ffffff;
@gray: #e0e0e0;
@gray-dark: #b1b1b1;
@large-padding:.1rem;
@small-padding: .1rem;

@icon-border-size: 1px;/*
@media screen and (min-width: 768px) {
  .__vev_calendar-wrapper{
    max-width: 1200px;
    margin: 0 auto;
    .cal-wrapper{
      width: 50%;
      padding: 100px 50px;
      .date-num{
        line-height: 50px;
      }
    }
    .events-wrapper{
      width: 50%;
      background-color: @base-orange;
      color: @white;
      padding: 40px 45px;
      position: absolute;
      left: 50%;
      top: 0;
      bottom: 0;
    }
  }
}
@media screen and (max-width: 767px) {
  .__vev_calendar-wrapper{
    .cal-wrapper{
      width: 100%;
      padding: 10px 5px;
      .date-num{
        line-height: 42px;
      }
    }
    .events-wrapper{
      width: 100%;
      margin-top: 10px;
      padding: 10px;
    }
  }
}*/
.__vev_calendar-wrapper{
  position: relative;
  overflow: hidden;
  width: 100%;
  *{
    box-sizing: border-box;
  }
  ::-webkit-scrollbar{
    width: 8px;
    height: 8px;
  }
  ::-webkit-scrollbar-track {
    box-shadow: inset 0 0 2px rgba(0,0,0,.2);
    border-radius: 5px;
  }
  ::-webkit-scrollbar-thumb {
    border-radius: 5px;
    background: rgba(0,0,0,.2);
  }
  .cal-wrapper{
    .cal-header{
      position: relative;
      width: 100%;
      background-color: @white;
      font-weight: 500;
      overflow: hidden;
      &>div{
        float: left;
        padding: @large-padding;
      }
      .title{
        width: 60%;
        text-align: center;
          font-size:.34rem;
      }
      .l{
        text-align: left;
        width: 20%;
        cursor: pointer;
        user-select: none;
        -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
      }
      .r{
        text-align: right;
        width: 20%;
        cursor: pointer;
        user-select: none;
        -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
      }
    }
    .cal-body{
      width: 100%;
      .weeks{
          width: 100%;
          overflow: hidden;
          text-align: center;
          font-size: .36rem;
          font-weight:500;
          margin-top: .24rem;
          margin-bottom: .5rem;
            .item {
                float: left;
                width: 14.285%;
            }
      }
      .dates{
        width: 100%;
        overflow: hidden;
        text-align: center;
        font-size: .4rem;
        .item{
            padding-bottom: .5rem;
          position: relative;
          float: left;
          display: block;
          width: 14.285%;
          cursor: default;
          -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
          .date-num{
            font-size: .3rem;
            position: relative;
            z-index: 3;
          }
        }
      }
    }
  }
  .arrow-left.icon {
    color: #000;
    position: absolute;
    left: 6%;
    margin-top: 10px;
  }

.arrow-left.icon:before {
    content: '';
    position: absolute;
    left: .01rem;
    top: -.1rem;
    width: .2rem;
    height: .2rem;
    border-top: solid @icon-border-size currentColor;
    border-right: solid @icon-border-size currentColor;
    -webkit-transform: rotate(-135deg);
    transform: rotate(-135deg);
}
  .arrow-right.icon {
    color: #000;
    position: absolute;
    right: 6%;
    margin-top: 10px;
  }
  .arrow-right.icon:before {
    content: '';
    position: absolute;
    right: .01rem;
    top: -.1rem;
    width: .2rem;
    height: .2rem;
    border-top: solid @icon-border-size currentColor;
    border-right: solid @icon-border-size currentColor;
    -webkit-transform: rotate(45deg);
            transform: rotate(45deg);
  }
  h3, p{
    margin: 0;
    padding: 0;
  }
}

</style>
