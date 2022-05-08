<template>
  <el-select v-model="v.id" filterable placeholder="搜索选择" @change="tt">
    <el-option v-for="item in teamsList" :key="item.id" :value="item.id" :label="item.name">{{item.name}}aa</el-option>
  </el-select>
</template>


<script>
export default {
  data() {
    global.t = this
    return {
      v: {},
      teamsList: [
        {
          "id": 1,
          "name": "巴萨队",
          "img": "https://i0.um.tcl.com/FtUeGr0H_qYb8lwjXR9TSakISDCR",
          "type": 1
        },
        {
          "id": 2,
          "name": "宇宙队",
          "img": "https://i0.um.tcl.com/FtUeGr0H_qYb8lwjXR9TSakISDCR",
          "type": 1
        },
        {
          "id": 12,
          "name": "ww",
          "img": "https://i0.um.tcl.com/FtUeGr0H_qYb8lwjXR9TSakISDCR",
          "type": 1
        }
      ]
    }
  },
  methods: {
    tt(a) {
      console.log(a)
    }
  }
}
</script>
