<!--
 * @Description:
 * @Author: cooky
 * @Date: 2020-01-08 09:31:28
 * @LastEditors  : cooky
 * @LastEditTime : 2020-01-09 16:21:22
 -->
<template>
  <div>
    <button @click="handleClick">重置数据</button>
    <RecycleScroller
      style="height:90vh"
      :items="list"
      :item-size="25"
      v-slot="{ item, index }"
      v-infinite-scroll="loadMore"
      infinite-scroll-disabled="busy"
      infinite-scroll-distance="10"
    >{{ item.value }}-{{ index }}</RecycleScroller>
  </div>
</template>

<script>
import { RecycleScroller } from 'vue-virtual-scroller'
import 'vue-virtual-scroller/dist/vue-virtual-scroller.css'

const defaultList = []
for (let i = 0; i < 20; i++) {
  defaultList.push({
    id: i + 1,
    value: 'item' + (i + 1)
  })
}
export default {
  components: { RecycleScroller },
  data() {
    global.t = this
    return {
      list: [],
      busy: false
    }
  },
  updated() {
    console.log('updated dsdfaesfsasawwd')
  },
  beforeUpdate() {
    console.log('beforeUpdate dsdfaesfsasawwd')
  },
  methods: {
    handleClick() {
      const otherList = []
      for (let i = 0; i < 20; i++) {
        otherList.push({
          id: 'other' + (i + 1),
          value: 'otheritem' + (i + 1)
        })
      }
      this.list = []
      this.$nextTick(() => {
        this.list = otherList
      })
    },
    loadMore: function () {
      console.log('load....')
      this.busy = true
      const length = this.list.length
      for (var i = length; i < length + 20; i++) {
        this.list.push({
          id: i + 1,
          value: 'item' + (i + 1)
        })
      }
      this.busy = false
    }
  }
}
</script>
