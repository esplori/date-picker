<template>
  <div class="date-picker">
    <div>
      <div class="year-month"><span>&lt;&lt;</span> <span @click="monthChangeDown">&lt;</span> {{currentYear}} 年{{currentMonth}} 月 <span>&gt;</span> <span>&gt;&gt;</span></div>
      <ul class="days-container">
        <li v-for="(item, index) in dayList" :key="index" class="single-day">{{item}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      dayList: [],
      currentYear: new Date().getFullYear(),
      currentMonth: new Date().getMonth() + 1
    }
  },
  mounted () {
    this.randerDayList()
  },
  methods: {
    monthChangeDown () {
      debugger
      this.currentMonth = --this.currentMonth
      this.randerDayList()
    },
    randerDayList () {
      let days = this.getCurrentMonthDays(this.currentYear, this.currentMonth)
      this.dayList = this.initDayList(days)
    },
    initDayList (days) {
      days = days || 0
      let list = []
      for (let i = 1; i <= days; i++) {
        list.push(i)
      }
      console.log(list)
      return list
    },
    getCurrentMonthDays (inputYear, inputMonth) {
      let currentDate = new Date()
      let year = inputYear || currentDate.getFullYear()
      let month = inputMonth || currentDate.getMonth() + 1 // 默认从0开始，所以需要加1
      let isRn = false
      let days = 0
      if (year % 400 === 0 || (year % 4 === 0 && year % 100 !== 0)) {
        // 判断是否为闰年：能被400整除或者能被4整除，但不能被100整除
        isRn = true
      } else {
        isRn = false
      }
      if (
        month === 1 ||
        month === 3 ||
        month === 5 ||
        month === 7 ||
        month === 8 ||
        month === 10 ||
        month === 12
      ) {
        // 1、3、5、7、8、10、12 每月31天
        days = 31
      } else if (month === 2) {
        if (isRn) {
          // 为闰年，则多一天为29天
          days = 29
        } else {
          days = 28
        }
      } else if (month === 4 || month === 6 || month === 9 || month === 11) {
        // 4、6、9、10、11
        days = 30
      }
      return days
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.date-picker{
  width: 50%;
  margin: 0 auto;
}
.days-container{
  width:370px;
  padding: 20px;
  border: 1px solid #ddd;
  text-align: left;
}
.single-day{
  display: inline-block;
  width: 29px;
  padding: 11px;
}
.year-month{
  width:370px;
  text-align: center;
}
</style>>
