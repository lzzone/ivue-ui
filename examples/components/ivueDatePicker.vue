<template>
  <div>
    <h1>日历变为横向 设置为多选 宽度百分百</h1>
    <IvueDatePicker v-model="dates" headerColor="red" color="blue" landscape fullWidth multiple></IvueDatePicker>

    <h1>一开始显示为月份</h1>
    <IvueDatePicker v-model="picker" type="month"></IvueDatePicker>

    <h1>语言</h1>
    <IvueDatePicker :value="picker" locale="zh-CH" :showCurrent="false"></IvueDatePicker>

    <h1>设置允许选择日期</h1>
    <IvueDatePicker v-model="picker" locale="zh-CH" :allowedDates="allowedDates"></IvueDatePicker>

    <h1>便签</h1>
    <p>function</p>
    <IvueDatePicker
      v-model="picker"
      :note="arrayEvents"
      :noteColor="date => date[9] % 2 ? 'red' : 'yellow'"
    ></IvueDatePicker>
    <p>array</p>
    <IvueDatePicker v-model="picker" :note="['2018-12-10']" :noteColor="'red'"></IvueDatePicker>

    <h1>一周的第一天</h1>
    <IvueDatePicker :value="picker" firstDayOfWeek="0"></IvueDatePicker>
    <IvueDatePicker :value="picker" firstDayOfWeek="1"></IvueDatePicker>

    <h1>图标设置</h1>
    <IvueDatePicker
      :value="picker"
      nextIcon="arrow_right"
      prevIcon="arrow_left"
      yearIcon="event_note"
    ></IvueDatePicker>

    <h1>只读</h1>
    <IvueDatePicker v-model="picker" readonly></IvueDatePicker>
    <IvueDatePicker v-model="picker" type="month" readonly></IvueDatePicker>

    <h1>noTitle</h1>
    <IvueDatePicker v-model="picker" noTitle @input="handleInput"></IvueDatePicker>

    <h1>width</h1>
    <IvueDatePicker v-model="picker" width="500"></IvueDatePicker>

    <h1>multiple</h1>
    <IvueDatePicker v-model="months" type="month" multiple></IvueDatePicker>

    <h1>max min</h1>
    <IvueDatePicker v-model="picker" min="2016-06-15" max="2018-03-20"></IvueDatePicker>

    <h1>reactive</h1>
    <IvueDatePicker v-model="picker" reactive></IvueDatePicker>
    <IvueDatePicker v-model="picker" locale="zh-CH" reactive :showCurrent="false"></IvueDatePicker>

    <h1>pickerDate</h1>
    <IvueDatePicker v-model="picker" :pickerDate.sync="pickerDate"></IvueDatePicker>

    <h1>titleDateFormat</h1>
    <IvueDatePicker
      v-model="picker"
      :titleDateFormat="format('zh-CN',{ weekday: 'short', month: 'short', day: 'numeric', timeZone: 'UTC' })"
    ></IvueDatePicker>

    <h1>titleYearFormat</h1>
    <IvueDatePicker
      v-model="picker"
      :titleYearFormat="titleYearFormat('zh-CN',{ year: 'numeric', timeZone: 'UTC' })"
    ></IvueDatePicker>

    <h1>dayFormat</h1>
    <IvueDatePicker
      v-model="picker"
      :dayFormat="format('zh-CN',{ day: 'numeric', timeZone: 'UTC' })"
    ></IvueDatePicker>

    <h1>monthFormat</h1>
    <IvueDatePicker
      v-model="picker"
      :monthFormat="format('zh-CN',{ month: 'short', timeZone: 'UTC' })"
    ></IvueDatePicker>

    <h1>headerDateFormat</h1>
    <IvueDatePicker
      v-model="picker"
      :headerDateFormat="format('zh-CN',{ month: 'long', year: 'numeric', timeZone: 'UTC' })"
    ></IvueDatePicker>
  </div>
</template>

<script>
export default {
  data () {
    return {
      picker: new Date().toISOString().substr(0, 10),
      dates: ['2018-09-15', '2018-09-20'],
      arrayEvents: null,
      months: ['2018-09', '2018-10'],
      pickerDate: null
    }
  },
  mounted () {
    this.arrayEvents = [...Array(6)].map(() => {
      const day = Math.floor(Math.random() * 30)
      const d = new Date()
      d.setDate(day)
      return d.toISOString().substr(0, 10)
    })
  },
  methods: {
    allowedDates: val => {
      return parseInt(val.split('-')[2], 10) % 2 === 0
    },
    handleInput (value) {
      console.log(value)
    },
    titleDateFormat (locale, options, { start, length } = { start: 0, length: 0 }) {
      // 适配IOS
      const makeIsoString = (dateString) => {
        const [year, month, date] = dateString.trim().split(' ')[0].split('-');


        return [year, month, date].join('-');
      }
      // 初始化 根据语言来格式化日期和时间的对象
      const intlFormatter = new Intl.DateTimeFormat(locale || undefined, options);

      return (dateString) => {
        return '12月12日周三'
      }

    },
    titleYearFormat (locale, options) {
      // 适配IOS
      const makeIsoString = (dateString) => {
        const [year, month, date] = dateString.trim().split(' ')[0].split('-');


        return [year, month, date].join('-');
      }
      // 初始化 根据语言来格式化日期和时间的对象
      const intlFormatter = new Intl.DateTimeFormat(locale || undefined, options);

      return (dateString) => {
        return '12月12日周三'
      }

    },
    format (locale, options) {
      // 适配IOS
      const makeIsoString = (dateString) => {
        const [year, month, date] = dateString.trim().split(' ')[0].split('-');

        return [year, this.padStart(month || 1, 2, 0), this.padStart(date || 1, 2, 0)].join('-');
      }
      // 初始化 根据语言来格式化日期和时间的对象
      const intlFormatter = new Intl.DateTimeFormat(locale || undefined, options);

      return (dateString) => {
        return intlFormatter.format(new Date(`${makeIsoString(dateString)}T00:00:00+00:00`))
      }

    },
    padStart (string, targetLength, padString) {
      targetLength = targetLength >> 0;
      string = String(string);
      padString = String(padString);
      if (string.length > targetLength) {
        return String(string);
      }

      targetLength = targetLength - string.length;

      if (targetLength > padString.length) {
        padString += padString.repeat(targetLength / padString.length);
      }

      return padString.slice(0, targetLength) + String(string);
    }
  },
  watch: {
    pickerDate (val) {
      console.log(val)
    }
  }
}
</script>
