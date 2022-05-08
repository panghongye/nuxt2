<template>
  <div>
    <div @click="showSelector">B:{{ selectorValue.text }}</div>

    <md-selector v-model="isSelectorShow" :data="data" large-radius @choose="onSelectorChoose"></md-selector>
  </div>
</template>

<script>
import { Selector } from 'mand-mobile'

export default {
  components: {
    [Selector.name]: Selector,
  },
  data() {
    return {
      isSelectorShow: false,
      data: [
        {
          value: '1',
          text: '选项一',
        },
        {
          value: '2',
          text: '选项二',
        },
        {
          value: '3',
          text: '选项三',
        },


      ],
      selectorValue: {},
    }
  },
  methods: {
    showSelector() {
      this.isSelectorShow = true
    },
    onSelectorChoose(value) {
      this.selectorValue = value
    },
  },
}

</script>
