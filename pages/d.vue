<template>
  <div>
    <p @click="isSelectorShow1 = true">
      A::
      {{ selectValue1.text }}
    <md-selector v-model="isSelectorShow1" :data="selectData" large-radius @choose="selectChoose1" />

    </p>
  </div>
</template>


<script>
import { Selector } from 'mand-mobile'
export default {
  components: {
    [Selector.name]: Selector,
  },
  data() {
    global.t = this
    return {
      isSelectorShow1: false,
      isSelectorShow2: false,
      selectData: [
        {
          text: '月度', value: 1,
          list: [
            { text: 1, value: 1 },
            { text: 2, value: 2 },
            { text: 3, value: 3 },
            { text: 4, value: 4 },
            { text: 5, value: 5 },
            { text: 6, value: 6 },
            { text: 7, value: 7 },
            { text: 8, value: 8 },
            { text: 9, value: 9 },
            { text: 10, value: 10 },
            { text: 11, value: 11 },
            { text: 12, value: 12 },
          ]
        },
        {
          text: '季度', value: 2,
          list: [
            { text: '一季度', value: 1 },
            { text: '二季度', value: 2 },
            { text: '三季度', value: 3 },
            { text: '四季度', value: 4 },
          ]
        },
        {
          text: '年度', value: 3,
          list: (function () {
            let arr = []
            for (let year = 2022; year <= 2030; year++) {
              arr.push({ text: year, value: year })
            }
            return arr
          })()
        },
      ],
      selectValue1: {},
      selectValue2: {},
    }
  },
  mounted() {
    // this.selectValue1 = this.selectData[0]
  },
  methods: {
    selectChoose1(value) { this.selectValue1 = value; console.log(value) },
    selectChoose2(value) { this.selectValue2 = value },
  }
};
</script>
