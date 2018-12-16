<template>
  <v-app id="dayspan" v-cloak v-if="updateApp"> 
    <ds-calendar-app ref="app"
      :calendar="calendar"
      :read-only="readOnly"
      :companyList="companyList"
      @change="saveState"
      @event-update="eventUpdate">

      <template slot="title">
        平纳科
      </template>
      <template slot="eventPopover" slot-scope="slotData">
         <ds-calendar-event-popover
          v-bind="slotData"
          :read-only="readOnly"
          @finish="saveState"
          @event-remove="eventRemove"
        ></ds-calendar-event-popover>
      </template>

      <template slot="eventCreatePopover" slot-scope="{placeholder, calendar, close}">
        <ds-calendar-event-create-popover
          :calendar-event="placeholder"
          :calendar="calendar"
          :close="$refs.app.$refs.calendar.clearPlaceholder"
          :companyList="companyList"
          @create-edit="$refs.app.editPlaceholder"
          @create-popover-closed="saveState"
          @event-create="eventCreate"
        ></ds-calendar-event-create-popover>
      </template>

      <!-- <template slot="eventTimeTitle" slot-scope="{calendarEvent, details}">
        <div>
          <v-icon class="ds-ev-icon"
            v-if="details.icon"
            size="14"
            :style="{color: details.forecolor}">
            {{ details.icon }}
          </v-icon>
          <strong class="ds-ev-title">{{ details.title }}</strong>
        </div>
        <div class="ds-ev-description">{{ getCalendarTime( calendarEvent ) }}</div>
      </template> -->

      <!-- <template slot="drawerBottom">
        <v-container fluid>
          <v-layout wrap align-center>
            <v-flex xs12>
              <v-checkbox box
                label="Read Only?"
                v-model="readOnly"
              ></v-checkbox>
            </v-flex>
            <v-flex xs12>
              <v-select
                label="Language"
                :items="locales"
                v-model="currentLocale"
                @input="setLocale"
              ></v-select>
            </v-flex>
          </v-layout>
        </v-container>
      </template> -->

    </ds-calendar-app>
  </v-app>
</template>

<script>
import {dsMerge} from 'doom-dayspan-vuetify/src/functions.js'
import { Calendar, CalendarEvent, Weekday, Month, Sorts, Schedule } from 'dayspan'
import * as moment from 'moment'

export default {

  name: 'dayspan',

  data: vm => ({
    storeKey: 'dayspanState',
    calendar: Calendar.months(),
    calendarEvent: CalendarEvent,
    updateAppIndex: null,
    updateApp: true,
    readOnly: false,
    currentLocale: vm.$dayspan.currentLocale,
    eventUpdateData: '',
    companyList:[
      {text : "上海市民公司",
       value : "上海市民公司11",
       id : "id",
       location : "徐汇区宜山路700号B2幢13楼",
      },
      {text : "上海市民公司1",
       value : "上海市民公司1111",
       id: 'haha22',
       location : "徐汇区宜山路700号B2幢13楼",
      },
      {text : "上海市民公司2",
       value : "上海市民公司1111111111",
       id: 'hah222a',
       location : "徐汇区宜山路700号B2幢13楼",
      },
      {text : "上海市民公司3",
       value : "上海复深蓝软件股份有限公司w",
       id: 'hah2222a',
       location : "徐汇区宜山路700号B2幢13楼",
      },
    ],    
    locales: [
      { value: 'en', text: 'English' },
      { value: 'fr', text: 'French' },
      { value: 'nl', text: 'Dutch' },
      { value: 'ca', text: 'Catalan' },
      { value: 'ch', text: 'China' },
    ],
    defaultEvents: [
      // {
      //   data: {
      //     title: 'Weekly Meeting',
      //     color: '#3F51B5'
      //   },
      //   schedule: {
      //     dayOfWeek: [Weekday.MONDAY],
      //     times: [9],
      //     duration: 30,
      //     durationUnit: 'minutes'
      //   }
      // },
      // {
      //   data: {
      //     title: 'First Weekend',
      //     color: '#4CAF50'
      //   },
      //   schedule: {
      //     weekspanOfMonth: [0],
      //     dayOfWeek: [Weekday.FRIDAY],
      //     duration: 3,
      //     durationUnit: 'days'
      //   }
      // },
      // {
      //   data: {
      //     title: 'End of Month',
      //     color: '#000000'
      //   },
      //   schedule: {
      //     lastDayOfMonth: [1],
      //     duration: 1,
      //     durationUnit: 'hours'
      //   }
      // },
      // {
      //   data: {
      //     title: 'Mother\'s Day',
      //     color: '#2196F3',
      //     calendar: 'US Holidays'
      //   },
      //   schedule: {
      //     month: [Month.MAY],
      //     dayOfWeek: [Weekday.SUNDAY],
      //     weekspanOfMonth: [1]
      //   }
      // },
      // {
      //   data: {
      //     title: 'New Year\'s Day',
      //     color: '#2196F3',
      //     calendar: 'US Holidays'
      //   },
      //   schedule: {
      //     month: [Month.JANUARY],
      //     dayOfMonth: [1]
      //   }
      // },
      // {
      //   data: {
      //     title: 'Inauguration Day',
      //     color: '#2196F3',
      //     calendar: 'US Holidays'
      //   },
      //   schedule: {
      //     month: [Month.JANUARY],
      //     dayOfMonth: [20]
      //   }
      // },
      // {
      //   data: {
      //     title: 'Martin Luther King, Jr. Day',
      //     color: '#2196F3',
      //     calendar: 'US Holidays'
      //   },
      //   schedule: {
      //     month: [Month.JANUARY],
      //     dayOfWeek: [Weekday.MONDAY],
      //     weekspanOfMonth: [2]
      //   }
      // },
      // {
      //   data: {
      //     title: 'George Washington\'s Birthday',
      //     color: '#2196F3',
      //     calendar: 'US Holidays'
      //   },
      //   schedule: {
      //     month: [Month.FEBRUARY],
      //     dayOfWeek: [Weekday.MONDAY],
      //     weekspanOfMonth: [2]
      //   }
      // },
      // {
      //   data: {
      //     title: 'Memorial Day',
      //     color: '#2196F3',
      //     calendar: 'US Holidays'
      //   },
      //   schedule: {
      //     month: [Month.MAY],
      //     dayOfWeek: [Weekday.MONDAY],
      //     lastWeekspanOfMonth: [0]
      //   }
      // },
      // {
      //   data: {
      //     title: 'Independence Day',
      //     color: '#2196F3',
      //     calendar: 'US Holidays'
      //   },
      //   schedule: {
      //     month: [Month.JULY],
      //     dayOfMonth: [4]
      //   }
      // },
      // {
      //   data: {
      //     title: 'Labor Day',
      //     color: '#2196F3',
      //     calendar: 'US Holidays'
      //   },
      //   schedule: {
      //     month: [Month.SEPTEMBER],
      //     dayOfWeek: [Weekday.MONDAY],
      //     lastWeekspanOfMonth: [0]
      //   }
      // },
      // {
      //   data: {
      //     title: 'Columbus Day',
      //     color: '#2196F3',
      //     calendar: 'US Holidays'
      //   },
      //   schedule: {
      //     month: [Month.OCTOBER],
      //     dayOfWeek: [Weekday.MONDAY],
      //     weekspanOfMonth: [1]
      //   }
      // },
      // {
      //   data: {
      //     title: 'Veterans Day',
      //     color: '#2196F3',
      //     calendar: 'US Holidays'
      //   },
      //   schedule: {
      //     month: [Month.NOVEMBER],
      //     dayOfMonth: [11]
      //   }
      // },
      // {
      //   data: {
      //     title: 'Thanksgiving Day',
      //     color: '#2196F3',
      //     calendar: 'US Holidays'
      //   },
      //   schedule: {
      //     month: [Month.NOVEMBER],
      //     dayOfWeek: [Weekday.THURSDAY],
      //     weekspanOfMonth: [3]
      //   }
      // },
      // {
      //   data: {
      //     title: 'Christmas Day',
      //     color: '#2196F3',
      //     calendar: 'US Holidays'
      //   },
      //   schedule: {
      //     month: [Month.DECEMBER],
      //     dayOfMonth: [25]
      //   }
      // }
    ]
  }),

  created () {
    this.updateAppIndex = setInterval(() => {
      this.updateApp = false
      this.$nextTick(() => {
        this.updateApp = true
      })
    }, 300000)
  },

  beforeDestroy () {
      clearInterval(this.updateAppIndex)
  },
  mounted()
  {
    window.app = this.$refs.app;

    this.loadState();
  },

  methods:
  {
    getCalendarTime(calendarEvent)
    {
      let sa = calendarEvent.start.format('a');
      let ea = calendarEvent.end.format('a');
      let sh = calendarEvent.start.format('h');
      let eh = calendarEvent.end.format('h');

      if (calendarEvent.start.minute !== 0)
      {
        sh += calendarEvent.start.format(':mm');
      }

      if (calendarEvent.end.minute !== 0)
      {
        eh += calendarEvent.end.format(':mm');
      }

      return (sa === ea) ? (sh + ' - ' + eh + ea) : (sh + sa + ' - ' + eh + ea);
    },

    setLocale(code)
    {
      moment.lang(code);

      this.$dayspan.setLocale(code);
      this.$dayspan.refreshTimes();

      this.$refs.app.$forceUpdate();
    },

    saveState(calendarEvent)
    {
      let state = this.calendar.toInput(true);
      // 支持增加和删除
      // console.log(state)
      // console.log(calendarEvent)
      if (calendarEvent.type && calendarEvent.type === 'remove') {
        // console.log(calendarEvent.event)
      } else if (calendarEvent.type && calendarEvent.type === 'creating') {
        // console.log(state.events)
      } else {
        if (this.eventUpdateData) {
          // console.log('修改数据', this.eventUpdateData)
          this.eventUpdateData = ''
        }
      }
      let json = JSON.stringify(state)

      localStorage.setItem(this.storeKey, json);
    },
    eventUpdate (event) {
      // 支持修改
      console.log('修改', event)
      this.eventUpdateData = event
    },
    eventRemove (event) {
      console.log(event)
    },
    eventCreate (event) {
      // console.log(event)
    },
    eventChange (event) {
      // console.log(event)
    },
    loadState()
    {
      let state = {};

      try
      {
        let savedState = JSON.parse(localStorage.getItem(this.storeKey));

        if (savedState)
        {
          state = savedState;
          state.preferToday = false;
        }
      }
      catch (e)
      {
        console.log( e );
      }

      if (!state.events || !state.events.length)
      {
        state.events = this.defaultEvents;
      }

      let defaults = this.$dayspan.getDefaultEventDetails();

      state.events.forEach(ev =>
      {
        ev.data = dsMerge( ev.data, defaults );
      })

      this.$refs.app.setState( state );
    }
  }
}
</script>

<style>

body, html, #app {
  width: 100%;
  height: 100%;
}

</style>
