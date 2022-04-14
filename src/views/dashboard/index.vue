<template>
  <div class="dashboard-container">
    <!-- <div class="dashboard-text">name: {{ name }}</div> -->
    <ele-calendar :render-content="renderContent" :data="datedef" border highlight @pick="clickpick" :prop="prop"></ele-calendar>
    <!-- selectionMode 日历模式设置了点击不了 -->
    <!-- currentmonth border highlight @select="select"  -->
    <!-- :selectionMode="'day'" -->
    <!-- @pick="clickpick"  -->
    <!--  -->
    <!--  -->
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import eleCalendar from 'ele-calendar'
import 'ele-calendar/dist/vue-calendar.css'
import _ from 'lodash'
import moment from 'moment'
export default {
  name: 'Dashboard',
  components: {
    eleCalendar
  },
  computed: {
    ...mapGetters([
      'name'
    ])
  },
  data() {
    return {
      datedef: [
        { "date": "2022-04-14", "content": "订餐", isselect: true },
        { "date": "2022-04-16", "content": "订餐", isselect: true },
        { "date": "2022-04-17", "content": "订餐", isselect: true },
        // { "date": "2022-04-18", "content": "订餐", isselect: false },
        // { "date": "2022-04-19", "content": "订餐", isselect: false },
        // { "date": "2022-04-20", "content": "订餐", isselect: false },
        // { "date": "2022-04-21", "content": "订餐", isselect: false },
        // { "date": "2022-04-22", "content": "订餐", isselect: false },
        // { "date": "2022-05-01", "content": "订餐", isselect: false },
        // { "date": "2023-05-01", "content": "订餐", isselect: false },
      ],
      prop: 'date'
    }
  },
  methods: {
    //点击禁用
    disabledDate(val) {
      var d = new Date(val)
      const month = d.getMonth() + 1
      const day = d.getDate()
      let smonth = month
      if (month < 10)
        smonth = '0' + month
      let sday = day
      if (sday < 10)
        sday = '0' + day
      var date = d.getFullYear() + '-' + smonth + '-' + sday
      return this.datedef.some((v => {
        if (v.date === date) {
          return true
        } else {
          return false
        }
      }))

    },
    renderContent(h, { defdate, ...parmas }) {
      // debugger;
      //把时间转成时间戳
      var imoment = moment(defdate);
      //把时间进行格式化
      let geshi = imoment.format('YYYY-MM-DD');
      //拿到时间所对应的星期
      let week = imoment.day();
      //用日期对比数组中的元素，如果存在就返回对象，如果不存在就返回空
      let info = _.find(this.datedef, { 'date': geshi });
      // :class=[]
      return (
        <div style="min-height:90px;"  >
          {imoment.date()}
          <br />
          {info ? info.content : ''}
        </div>
      );

    },
    // renderContent(h, parmas) {
    //   console.log(parmas)
    //   debugger;
    //   const loop = data => {

    //     debugger;
    //     return (
    //       data.defvalue.value ? (<div><div>{data.defvalue.text}</div>
    //         <span  >备选项</span>
    //       </div>) : <div>{data.defvalue.text}</div>
    //     )
    //   }
    //   return (
    //     <div style="min-height:60px;">
    //       {loop(parmas)}
    //     </div>
    //   );
    // },
    //点击日历	返回当前点击时间data、event、row、dome
    // clickpick(value, event, row, celltd) {
    //   console.log(value)
    //   debugger;
    //   var d = new Date(value)
    //   const month = d.getMonth() + 1
    //   const day = d.getDate()
    //   let smonth = month
    //   if (month < 10)
    //     smonth = '0' + month
    //   let sday = day
    //   if (sday < 10)
    //     sday = '0' + day

    //   var date = d.getFullYear() + '-' + smonth + '-' + sday
    //   console.log(date)
    // },
    clickpick(date, event, row, dome) {
      //如果点击的是今天也要让
      if (row.type == 'today') {
        row.type = 'normal'
        debugger;
      }
      // 对时间格式进行格式化，拿到年月日和周
      // 查找点击日期，是否在数组里面，如果是返回一个对象，如果没有返回空
      // 数组存在这一对象，则修改，如果不存在，则添加到数组
      let imoment = moment(date);
      // 格式化成，年月日
      let iformat = imoment.format('YYYY-MM-DD');
      // 格式化，拿到日
      let idate = imoment.date();
      // 拿到星期
      let week = imoment.day();
      // 在数组中查找，数组中是否有点击日期对应的对象，有则拿出来，没有接返回空
      let info = _.find(this.datedef, { 'date': iformat });
      // 在判断在数组中有没有这个对象，如果有，修改属性，没有，在数组中加入一个对象
      console.log(info);
      if (info) {
        this.datedef = info ? _.map(this.datedef, iobject => info == iobject ? { ...iobject, 'content': iobject.content == '订餐' ? '' : '订餐', 'isselect': iobject.content == '订餐' ? 'true' : 'false' } : iobject) : ''
        //:[...this.datedef,{'date':iformat,'content':[0,6].indexOf(week)!=-1?'订餐':''}];
      } else {
        this.datedef = info ? _.map(this.datedef, iobject => info == iobject ? { ...iobject, 'content': iobject.content == '' ? '订餐' : '', 'isselect': true } : iobject) : [...this.datedef, { 'date': iformat, 'content': info ? '' : '订餐', 'isselect': info ? 'false' : 'true' }];
      }
      this.datedef = this.datedef.filter(item => item.content != '')
      console.log(this.datedef)
    },
    select(val, selectDom) {
      console.log(val, selectDom)
      selectDom.disabled = true
    }
  }
}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
