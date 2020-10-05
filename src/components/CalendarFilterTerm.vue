<template>
  <div class="calendar-filter__term">
    <div class="list">
      <div
      class="list__item"
      >
      Весь срок
      </div>
      <div
      v-for="(item, index) in listTerm"
      class="list__item"
      :class="{list__item_active:indexListItemActive===index}"
      :key="index" 
      @click="onClick(index)"
      >
      {{item}}  
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CalendarFilter',
  data() {
    return {
      listTerm: ['Сегодня','Вчера','Последние 7 дней','Последние 30 дней','В этом месяце', 'Прошлый месяц'],
      indexListItemActive: null
    }
  },
  methods: {
    initialize() {
      this.setListItemStyleActive(0)  
      this.$emit('click',1)
    },
    onClick(index) {
      this.setListItemStyleActive(index)
      this.$emit('index',index+1)
      this.$emit('nameTerm', this.listTerm[index])
    },

    setListItemStyleActive(index) {
      this.indexListItemActive = index
    }
  },
  mounted() {
    this.initialize() 
  }
}
</script>

<style lang="scss" scoped>
.calendar-filter {
  &__term {
    width: 176px;
    background: #FFFFFF;
    box-shadow: 0px 10px 40px rgba(128, 158, 191, 0.2);
    border-radius: 10px;
    margin-right: 8px;
    .list {
      font-family: 'PT Sans';
      font-style: normal;
      font-weight: normal;
      font-size: 15px;
      line-height: 22px;
      margin-left: 10px;
      padding-top: 12px;
      padding-bottom: 47px;
      cursor: pointer;
      &__item {
        box-sizing: border-box;
        width: 156px;
        padding-top: 10px;
        padding-bottom: 7px;
        padding-left: 10px;
        padding-right: 10px;
        &_active {
          background: #F0F3F8;
          border-radius: 5px;
        }
        &:hover:not(:nth-child(1)) {
          box-shadow: 0 0 0 2px #EBEEF3;
          border-radius: 5px;
          width: 154px;
        }
      }  
    } 
  }
}
</style>
