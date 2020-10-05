<template>
  <div class="calendar-filter"
  :class="{'calendar-filter_active':isOpenCalendar}"
  >
    <div class="calendar-filter__menu"
    >
    <button class="button" @click="isOpenCalendar=true">
      <svg class="button__icon-left" width="23" height="20" viewBox="0 0 23 20" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M21.5001 20H1C0.4 20 0 19.6 0 19V2.70001C0 2.10001 0.4 1.70001 1 1.70001H21.5001C22.1001 1.70001 22.5001 2.10001 22.5001 2.70001V19C22.5001 19.5 22.1001 20 21.5001 20ZM2 18H20.5001V3.70001H2V18Z" fill="black"/>
        <path d="M17.7 5.29999C17.1 5.29999 16.7 4.89999 16.7 4.29999V1C16.7 0.4 17.1 0 17.7 0C18.3 0 18.7 0.4 18.7 1V4.29999C18.7 4.89999 18.3 5.29999 17.7 5.29999Z" fill="black"/>
        <path d="M5.00006 5.29999C4.40006 5.29999 4 4.89999 4 4.29999V1C4 0.4 4.40006 0 5.00006 0C5.60006 0 6.00006 0.4 6.00006 1V4.29999C6.00006 4.89999 5.50006 5.29999 5.00006 5.29999Z" fill="black"/>
        <path d="M21.5001 8.60004H1.20001C0.900012 8.60004 0.700012 8.40004 0.700012 8.10004C0.700012 7.80004 0.900012 7.60004 1.20001 7.60004H21.5001C21.8001 7.60004 22.0001 7.80004 22.0001 8.10004C22.0001 8.30004 21.8001 8.60004 21.5001 8.60004Z" fill="black"/>
        <path d="M5.70001 10.6H3.70001V12.5H5.70001V10.6Z" fill="black"/>
        <path d="M9.00006 10.6H7.10004V12.5H9.00006V10.6Z" fill="black"/>
        <path d="M12.4 10.6H10.5001V12.5H12.4V10.6Z" fill="black"/>
        <path d="M15.8 10.6H13.8V12.5H15.8V10.6Z" fill="black"/>
        <path d="M19.1 10.6H17.2V12.5H19.1V10.6Z" fill="black"/>
        <path d="M5.70001 13.7H3.70001V15.7H5.70001V13.7Z" fill="black"/>
        <path d="M9.00006 13.7H7.10004V15.7H9.00006V13.7Z" fill="black"/>
        <path d="M12.4 13.7H10.5001V15.7H12.4V13.7Z" fill="black"/>
        <path d="M15.8 13.7H13.8V15.7H15.8V13.7Z" fill="black"/>
        <path d="M19.1 13.7H17.2V15.7H19.1V13.7Z" fill="black"/>
      </svg>
      {{name}}
      <svg class="button__icon-right" width="11" height="6" viewBox="0 0 11 6" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M5.5 6C5.21552 6 5.02591 5.91178 4.83625 5.7353L0.284483 1.50002C-0.0948276 1.14708 -0.0948276 0.617647 0.284483 0.264706C0.663793 -0.0882353 1.23278 -0.0882353 1.61209 0.264706L5.5 3.88239L9.38791 0.264706C9.76722 -0.0882353 10.3362 -0.0882353 10.7155 0.264706C11.0948 0.617647 11.0948 1.14708 10.7155 1.50002L6.16375 5.7353C5.97409 5.91178 5.68965 6 5.5 6Z" fill="black"/>
      </svg>
    </button>
    <div v-if="isOpenTerm" class="term">
      {{date[0].month}}.{{date[0].number}}.{{date[0].year}}
      {{date[1].month?" - "+date[1].month+".":""}}{{date[1].number?date[1].number+".":""}}{{date[1].year+" г."}}  
    </div>
    <div v-if="isOpenCalendar" class="menu-open">
      <CalendarFilterTerm
      @nameTerm="name=$event" @index="termIndex=$event"/>
      <CalendarFilterDate
      @closeCalendar="closeCalendar"
      @sendDate="sendDate"
      :termIndex="termIndex"/>
      </div>
    </div>
  </div>
</template>

<script>
import CalendarFilterTerm from './CalendarFilterTerm'
import CalendarFilterDate from './CalendarFilterDate'

export default {
  name: 'CalendarFilter',
  data() {
    return {
      date: null,
      termIndex: 1,
      name: 'Сегодня',
      isOpenCalendar: false,
      isOpenTerm: false,
    }
  },
  methods: {
    sendDate(e) {
      this.date=e
      this.isOpenCalendar=false
      this.isOpenTerm=true
    },
    closeCalendar(e) {
      this.isOpenCalendar=e
      this.isOpenTerm=true
    }
  },
  components: {
    CalendarFilterTerm,
    CalendarFilterDate
  }
}
</script>

<style lang="scss" scoped>
.calendar-filter {
  display: flex;
  &_active {
    &::before {
      content: '';
      width: 100%;
      height: 100%;
      position: absolute;
      background: rgba(240, 246, 252, 0.7);
      backdrop-filter: blur(4px);
      z-index: 1;  
    }
  }
  &__menu {
    position: relative;
  }
  .term {
    font-family: 'PT Sans';
    font-style: normal;
    font-weight: bold;
    font-size: 15px;
    line-height: 150%;
    width: 223px;
    display: flex;
    justify-content: center;
    background: #586375;
    border-radius: 100px;
    margin-top: 20px; 
    padding-top:9px;
    padding-bottom:11px; 
    color: white;
  }
  .menu-open {
    display: flex;
    position: absolute;
    top: 10px;

    padding-top: 20px;
    z-index: 1;  
  }
  .button {
    display: flex;
    align-items: center;
    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 500;
    font-size: 15px;
    line-height: 130%;

    &__icon-left {
      margin-right: 19.5px;
      
    }
    &__icon-right {
      padding-top: 4px;
      margin-left: 8px;
    } 

    &:hover {
      color: #FF7439; 
    } 
    &:hover path {
      fill: #FF7439; 
    }
    &:hover .button__icon-right  {
      margin-top: 6px;
      transform: rotate(180deg);
    }
  }
}
</style>
