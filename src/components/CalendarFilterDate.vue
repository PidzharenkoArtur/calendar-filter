<template>
  <div class="calendar-filter__date">
    <div class="top">
      <button class="top__button" @click="showPrevMonth()">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
          <path d="M11.7499 23.5C8.64993 23.5 5.65 22.3 3.45 20.1C-1.15 15.5 -1.15 8.00002 3.45 3.40002C5.65 1.20002 8.64993 0 11.7499 0C14.8499 0 17.85 1.20002 20.05 3.40002C22.25 5.60002 23.55 8.60001 23.55 11.7C23.55 14.8 22.35 17.8 20.15 20C17.85 22.2 14.8499 23.5 11.7499 23.5ZM11.7499 1.90002C9.14993 1.90002 6.6499 2.89999 4.8499 4.79999C1.0499 8.59999 1.0499 14.8 4.8499 18.6C6.6499 20.4 9.14993 21.5 11.7499 21.5C14.3499 21.5 16.85 20.5 18.65 18.6C20.45 16.8 21.55 14.3 21.55 11.7C21.55 9.10001 20.55 6.59999 18.65 4.79999C16.75 2.89999 14.3499 1.90002 11.7499 1.90002Z" fill="black"/>
          <path d="M14.1499 17.5C13.8499 17.5 13.65 17.4 13.45 17.2L8.6499 12.4C8.4499 12.2 8.34985 12 8.34985 11.7C8.34985 11.4 8.4499 11.2 8.6499 11L13.45 6.20002C13.85 5.80002 14.4499 5.80002 14.8499 6.20002C15.2499 6.60002 15.2499 7.19999 14.8499 7.59999L10.7499 11.7L14.8499 15.8C15.2499 16.2 15.2499 16.8 14.8499 17.2C14.6499 17.4 14.3499 17.5 14.1499 17.5Z" fill="black"/>
        </svg>
      </button>
      <div>
        {{currentMonth}}, {{currentYear}}
      </div>
      <button class="top__button" @click="showNextMonth()">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M11.8001 1.02721e-06C14.9001 7.56201e-07 17.9 1.20002 20.1 3.40002C24.7 8.00002 24.7 15.5 20.1 20.1C17.9 22.3 14.9001 23.5 11.8001 23.5C8.70012 23.5 5.70007 22.3 3.50007 20.1C1.30007 17.9 7.17274e-05 14.9 7.14564e-05 11.8C7.11854e-05 8.69999 1.20009 5.7 3.4001 3.5C5.70009 1.3 8.70012 1.29822e-06 11.8001 1.02721e-06ZM11.8001 21.6C14.4001 21.6 16.9001 20.6 18.7001 18.7C22.5001 14.9 22.5001 8.70002 18.7001 4.90002C16.9001 3.10003 14.4001 2 11.8001 2C9.20012 2 6.70009 3.00003 4.9001 4.90003C3.1001 6.70003 2.00007 9.19999 2.00007 11.8C2.00007 14.4 3.0001 16.9 4.9001 18.7C6.8001 20.6 9.20012 21.6 11.8001 21.6Z" fill="black"/>
          <path d="M9.40015 5.99999C9.70015 5.99999 9.9001 6.09998 10.1001 6.29998L14.9001 11.1C15.1001 11.3 15.2002 11.5 15.2002 11.8C15.2002 12.1 15.1001 12.3 14.9001 12.5L10.1001 17.3C9.7001 17.7 9.1002 17.7 8.7002 17.3C8.3002 16.9 8.3002 16.3 8.7002 15.9L12.8002 11.8L8.70019 7.7C8.30019 7.3 8.30019 6.69998 8.70019 6.29998C8.90019 6.09998 9.20015 5.99999 9.40015 5.99999Z" fill="black"/>
        </svg>
      </button>
    </div>
    <div class="body">
      <div
      class="body__days-week"
      >
        <div 
        v-for="day in calendar.daysWeek"
        :key="day" 
        >
        {{day}}
        </div>
      </div>
      <div class="body__numbers-month">
        <div
        v-for="(numberRow, i) in numbersMonth"
        :key="i"
        class="row" 
        >
          <span
          v-for="(number, j) in numberRow"
          @click="onClick(number,i,j)"
          :key="j"
          :class="{
          'item':!!number, 
          'item_empty':!number, 
          'item_active': isItemActive(number,i,j), 
          'item_term': isItemTerm(number),
          'item_active-start': isItemActiveStart(number),
          'item_active-finish': isItemActiveFinish(number),
          'item_active-only-start': isItemActiveOnlyStart()
          }" 
          >
            {{number}} 
          </span>  
        </div>
      </div> 
    </div>
     <!-- calendar.item.j===j && calendar.item.i===i -->
    <div class="bottom">
      <button @click="$emit('closeCalendar')" class="bottom__button bottom__button_cancel">Отмена</button>
      <button 
      @click="update" 
      class="bottom__button bottom__button_update"
      :class="{'bottom__button_active':calendar.isActiveButtonUpdate}"
      >Обновить</button>
    </div>
  </div>
</template>

<script>
import moment from 'moment'
export default {
  name: 'CalendarFilter',
  props: {
    termIndex: {
      type: Number  
    }
  },
  data() {
    return {
      calendar: {
        daysWeek: ['Пн','Вт','Ср','Чт','Пт','Сб','Вс'],
        diapason: {},
        diapasonStartFinish: {},
        setDate: {
          0:{},
          1:{}
        },
        year: moment().year(),
        month: moment().month()+1,
        listMonth: {
          1: "Январь",
          2: "Февраль",
          3: "Март",
          4: "Апрель",
          5: "Май",
          6: "Июнь",
          7: "Июль",
          8: "Август",
          9: "Сентябрь",
          10: "Октябрь",
          11: "Ноябрь",
          12: "Декабрь",
        },
        termMonthNow: null,
        termMonthPrev: null,
        termYearNow: null,
        lastDay7: 6,
        lastDay30: 29,
        maxNumber: 31,
        item: {},
        isYourClick: false,
        term: {
          0: ()=> {
            return false
          },
          1:()=> {
            return this.termToday()
          },
          2:()=> { 
            return this.termYesterday()
          },
          3: ()=> {
            return this.termLastDays(this.calendar.lastDay7,this.last7DayNumber)
          }, 
          4: ()=> {
            return this.termLastDays(this.calendar.lastDay30,this.last30DayNumber)
          }, 
          5: ()=> {
            return this.termThisMonth()
          },
          6: ()=> {
            return this.termLastMonth()
          }
        },
        isActiveButtonUpdate: false
      },
      count: 0,
    }
  },
  watch: {
    termIndex(i) {
      this.startDate()
      this.calendar.term[i]()
    }  
  },
  computed: {
    todayNumber() {
      return +moment(new Date()).format('D')
    },
    yesterdayNumber() {
      return +moment(new Date()).add(-1,'days').format('D')
    },
    last7DayNumber() {
      return +moment(new Date()).add(-this.calendar.lastDay7,'days').format('D')
    },
    last30DayNumber() {
      return +moment(new Date()).add(-this.calendar.lastDay30,'days').format('D')
    },
    date() {
      return this.calendar.year + '-' + this.calendar.month
    },
    currentMonth() {
      return this.calendar.listMonth[this.calendar.month]
    },
    currentYear() {
      return this.calendar.year
    },
    countDaysInMonth() {
      return moment(this.date, "YYYY-MM").daysInMonth()
    },
    dayWeekFirstNumber() {
      let dayWeekNumber = moment(this.date, "YYYY-MM").date(1).format('d') 
      return +dayWeekNumber===0?7:dayWeekNumber
    },
    numbersMonth() {
      let i, arraynNumbersMonth = [],
      matrixNumbersMonth = []
      
      for (i = 1; i <= this.countDaysInMonth; i++) {
        arraynNumbersMonth.push(i)  
      }
      for (i = 1; i < this.dayWeekFirstNumber; i++) {
        arraynNumbersMonth.unshift(null)  
      }
      for (i = 0; i <= 42-this.countDaysInMonth-this.dayWeekFirstNumber; i++) {
        arraynNumbersMonth.push(null)  
      }
      for (i = 0; i < 6; i++) {
        matrixNumbersMonth.push(arraynNumbersMonth.splice(0,7))
      }

      if (matrixNumbersMonth[matrixNumbersMonth.length-1][0] === null) {
        matrixNumbersMonth.splice(matrixNumbersMonth.length-1, 1)
      }
      return matrixNumbersMonth
    },
    daysMonth() {
      let days = [], i;

      for (i = 1; i <= this.countDaysInMonth; i++) {
        days.push(moment(this.date, "YYYY-MM").date(i))
      }

      return days
    },
    daysWeekNumbers() {
      return this.daysMonth.reduce((obj, day)=> {
        if (!Object.keys(obj).includes(day.format('d'))) {
          obj[day.format('d')] = []
        }
        obj[day.format('d')].push(day.format('D'))

        return obj
      }, {})
    }  
  },
  mounted() {
    this.calendar.term[this.termIndex]()
  },
  methods: {
    update() {
      this.$emit('sendDate',this.calendar.setDate)
    },
    onClick(n,i,j) {
      this.calendar.isYourClick = true
      this.calendar.isActiveButtonUpdate = false
      this.$set(this.calendar.item,'i',i)
      this.$set(this.calendar.item,'j',j)
      this.$set(this.calendar.item,'number',n)
      this.$set(this.calendar.item,'month',this.calendar.month)
      this.$set(this.calendar.item,'year',this.calendar.year)

      this.calendar.termMonthNow = this.calendar.month
      this.calendar.termYearNow = this.calendar.year  
      
      if (this.count>1) {
        this.count = 0
        this.calendar.diapasonStartFinish = {}
      }

      this.$set(this.calendar.diapasonStartFinish,this.count,{...this.calendar.item})

      if (this.count === 1 && moment(this.calendar.diapasonStartFinish[0].year+'-'+this.calendar.diapasonStartFinish[0].month+'-'+this.calendar.diapasonStartFinish[0].number).isAfter(this.calendar.diapasonStartFinish[1].year+'-'+this.calendar.diapasonStartFinish[1].month+'-'+this.calendar.diapasonStartFinish[1].number)) {
        let first = {...this.calendar.diapasonStartFinish[0]}
        let second = {...this.calendar.diapasonStartFinish[1]}
        this.calendar.diapasonStartFinish[0] = second
        this.calendar.diapasonStartFinish[1] = first

      }

      if (this.count === 1 ) {
        this.calendar.isActiveButtonUpdate = true
        this.calendar.setDate = this.calendar.diapasonStartFinish
      }

      this.count++
    },
    startDate() {
      this.calendar.year = moment().year()
      this.calendar.month = moment().month()+1
      this.calendar.isYourClick = false

    },
    isItemActive(number,i,j) {
      if (this.calendar.isYourClick) {
        return (this.calendar.diapasonStartFinish[0].j===j && this.calendar.diapasonStartFinish[0].i===i
        ||this.calendar.diapasonStartFinish[1]&&this.calendar.diapasonStartFinish[1].j===j && this.calendar.diapasonStartFinish[1].i===i
        )
        && !!number
        && (this.calendar.termMonthNow === this.calendar.month)
        && this.calendar.termYearNow === this.calendar.year  
      }
      
      return ((number===this.calendar.diapason.start
      ||number===this.calendar.diapason.finish)
      && !!number
      && (this.calendar.month === this.calendar.termMonthPrev || this.calendar.month === this.calendar.termMonthNow)
      && this.calendar.termYearNow === this.calendar.year
      )
    },
    isItemTerm(number) {
      if (this.calendar.isYourClick) {
        return number>=this.calendar.diapasonStartFinish[0].number 
        && this.calendar.diapasonStartFinish[1] && number<=this.calendar.diapasonStartFinish[1].number
        && !!number
        && (this.calendar.termMonthNow === this.calendar.month)
        && this.calendar.termYearNow === this.calendar.year  
      }
      return (number>=this.calendar.diapason.start
      &&number<=this.calendar.diapason.finish 
      && !!number
      && (this.calendar.month === this.calendar.termMonthPrev || this.calendar.month === this.calendar.termMonthNow)
      && this.calendar.termYearNow === this.calendar.year
      )
    },
    isItemActiveStart(number) {
      if (this.calendar.isYourClick) {
        return (number===this.calendar.diapasonStartFinish[0].number )
      }
      return (number===this.calendar.diapason.start)
    },
    isItemActiveFinish(number) {
      if (this.calendar.isYourClick) {
        return (this.calendar.diapasonStartFinish[1]&&number===this.calendar.diapasonStartFinish[1].number )
      }
      return (number===this.calendar.diapason.finish)
    },
    isItemActiveOnlyStart() {
      if (this.calendar.isYourClick) {
        if (this.calendar.diapasonStartFinish[1]) {
          return this.calendar.diapasonStartFinish[1].number === this.calendar.diapasonStartFinish[0].number
        } else {
          return this.calendar.diapason.start === this.calendar.diapason.finish  
        }
      }
      return this.calendar.diapason.start === this.calendar.diapason.finish
    },
    showPrevMonth() {
      if (--this.calendar.month === 0) {
        this.calendar.year--
        this.calendar.month = 12 
      }

      this.updateDiapason()
    },
    showNextMonth() {
      if (++this.calendar.month > 12) {
        ++this.calendar.year
        this.calendar.month = 1 
      }

      this.updateDiapason()
    },
    updateDiapason() {
      if (this.termIndex!==3 && this.termIndex!==4 ) return
      
      let lastDaysFirstNumber, number;
      if (this.termIndex===3) {
        number = this.calendar.lastDay7
        lastDaysFirstNumber = this.last7DayNumber
      }
      if (this.termIndex===4) {
        number = this.calendar.lastDay30
        lastDaysFirstNumber = this.last30DayNumber
      }
      if (this.calendar.month===this.calendar.termMonthNow) {
        let n = this.todayNumber-number 
        if (n < 0) {
          this.calendar.diapason = {
            start: n,
            finish: this.todayNumber
          }  
        } 
        else {
          this.calendar.diapason = {
            start: n,
            finish: this.todayNumber
          } 
        }
      }
      if (this.calendar.month===this.calendar.termMonthPrev) {
        this.calendar.diapason = {
          start: lastDaysFirstNumber,
          finish: this.calendar.maxNumber
        }   
      }
    },
    termToday() {
      this.calendar.termMonthNow = this.calendar.month
      this.calendar.termYearNow = this.calendar.year
      this.calendar.diapason = {
        start: this.todayNumber,
        finish: this.todayNumber
      }
      this.calendar.setDate = {
        0:{},
        1:{}  
      }
      this.calendar.setDate['0'].number = this.todayNumber
      this.calendar.setDate['0'].month = this.calendar.termMonthNow
      this.calendar.setDate['0'].year = this.calendar.termYearNow
    },
    termYesterday() {
      this.termSettings(this.yesterdayNumber)

      this.calendar.diapason = {
        start: this.yesterdayNumber,
        finish: this.yesterdayNumber
      }
      this.calendar.setDate = {
        0:{},
        1:{}  
      }
      this.calendar.setDate['0'].number = this.yesterdayNumber
      this.calendar.setDate['0'].month = this.calendar.termMonthNow
      this.calendar.setDate['0'].year = this.calendar.termYearNow

      this.calendar.isActiveButtonUpdate = true
    },
    termLastDays(number, lastDayNumber) {
      let start, finish, n

      n = this.todayNumber-number 

      start = n
      finish = this.todayNumber
      
      if (this.termSettings(lastDayNumber)) {
        start = lastDayNumber
        finish = this.calendar.maxNumber
      }

      this.calendar.diapason = {
        start: start,
        finish: finish
      }
      this.calendar.setDate = {
        0:{},
        1:{}  
      }
      this.calendar.setDate['0'].number = lastDayNumber
      this.calendar.setDate['0'].month = this.calendar.termMonthPrev
      this.calendar.setDate['0'].year = this.calendar.termYearNow

      this.calendar.setDate['1'].number = this.todayNumber
      this.calendar.setDate['1'].month = this.calendar.termMonthNow
      this.calendar.setDate['1'].year = this.calendar.termYearNow

      this.calendar.isActiveButtonUpdate = true
    },

    termThisMonth() {
      if(this.termSettings(this.countDaysInMonth)) {
        this.showNextMonth()
        this.calendar.termMonthPrev = this.calendar.termMonthNow 
      }
      this.calendar.diapason = {
        start: 0,
        finish: this.countDaysInMonth
      }
      this.calendar.setDate = {
        0:{},
        1:{}  
      }
      this.calendar.setDate['0'].number = 1
      this.calendar.setDate['0'].month = this.calendar.termMonthNow
      this.calendar.setDate['0'].year = this.calendar.termYearNow

      this.calendar.setDate['1'].number = this.countDaysInMonth
      this.calendar.setDate['1'].month = this.calendar.termMonthNow
      this.calendar.setDate['1'].year = this.calendar.termYearNow
      
      this.calendar.isActiveButtonUpdate = true
    },

    termLastMonth() {
      if(this.termSettings(this.countDaysInMonth)) {
        this.calendar.termMonthNow = this.calendar.termMonthPrev 
      }
      this.calendar.diapason = {
        start: 0,
        finish: this.countDaysInMonth
      }
      this.calendar.setDate = {
        0:{},
        1:{}  
      }
      this.calendar.setDate['0'].number = 1
      this.calendar.setDate['0'].month = this.calendar.termMonthNow
      this.calendar.setDate['0'].year = this.calendar.termYearNow

      this.calendar.setDate['1'].number = this.countDaysInMonth
      this.calendar.setDate['1'].month = this.calendar.termMonthNow
      this.calendar.setDate['1'].year = this.calendar.termYearNow
      
      this.calendar.isActiveButtonUpdate = true
    },

    termSettings(number) {
      this.calendar.termMonthNow = this.calendar.month
      this.calendar.termYearNow = this.calendar.year
      if (number>this.todayNumber) {
        this.showPrevMonth()
        this.calendar.termMonthPrev = this.calendar.month
        return true
      }  
    }
  }
}
</script>

<style lang="scss" scoped>

.calendar-filter {
  &__date {
    width: 288px;
    background: #FFFFFF;
    box-shadow: 0px 10px 40px rgba(128, 158, 191, 0.2);
    border-radius: 10px;

    .top {
      display: flex;
      justify-content:space-between;
      font-size: 16px;
      line-height: 21px;
      padding: 7px;
      padding-top: 19px;
      padding-bottom: 15.5px;

      &__button {
          height: 0;

        &:hover {
          cursor: pointer;
          color:  #FF7439;
        }
      }  
    }

    .body {
      background: #F3F8FD;
      font-family: 'Montserrat';
      padding-left: 15px;
      padding-right: 15px;
      padding-bottom: 20px;
      &__days-week {
        display: flex;
        justify-content:space-between;
        font-style: normal;
        font-weight: bold;
        font-size: 12px;
        line-height: 15px;
        padding-bottom: 6px;
        padding-top: 17px; 
      }
      &__numbers-month {
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 400;
        font-size: 12px;
        line-height: 15px;

        .row {
          display: flex;
          margin-left: -6px;
          margin-bottom: 2px;
          justify-content: space-between;
          .item {
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            width: 28px;
            height: 28px;
            text-align: center;            
            box-sizing: border-box;
            margin-right: -6px;
            border-radius: 50%;

            &:hover {
              background: #DFE9F3;
            } 
            &_empty {
              width: 28px;
              height: 28px;
              margin-right: -6px;
            }
            &_active {
              background: #FF7439;
              color: white;
            }
            &_term {
              position: relative; 
              &::after {
                content: '';
                position: absolute;   
                background: rgba(255, 116, 57, 0.2);
                width: 40.34px;
                height: 30px;
              }
              &:first-child {
                &::after {
                content: '';
                  left: -9px;
                  width: 43px;
                }
              }
              &:last-child {
                &::after {
                content: '';
                  right: -9px;
                  width: 43.2px;
                }
              }
            }
            &_active-start {
              &::after {
                content: '';
                left: 9px!important;
                width: 25px!important;
              }
            }
            &_active-finish {
              &::after {
                content: '';
                left: -6.16px !important;
                width: 22px!important;
              }
            }
            &_active-only-start {
              &::after {
                width: 0!important;
              }  
            }
          }
        }
      } 
    }

    .bottom {
      display: flex;
      justify-content:space-between;
      font-family: 'Montserrat';
      padding:10px;
      &__button {
        width: 130px;
        padding: 13px;
        font-style: normal;
        font-weight: 600;
        font-size: 12px;
        line-height: 150%;
        &_cancel {
          padding: 11px;
          border: 2px solid #000000;
          border-radius: 7px;
          &:hover {
            background: #000000;
            color: white;
          }
        } 
        &_update {
          color: white;
          background: rgba(255, 116, 57, 0.5);
          border-radius: 7px;
        }
        &_active {
          background: #FF7439;
        }  
      }
    }
  }
}
</style>
