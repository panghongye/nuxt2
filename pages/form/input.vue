<template>
  <el-input v-model="vModel" :rows="rows || 5" :type="type">
    <template v-if="prepend" slot="prepend">{{ prepend }}</template>
    <template v-if="append" slot="append">{{ append }}</template>
  </el-input>
</template>

<script>
export default {
  name: "input",
  props: {
    type: { type: String, default: "input" }, // 控件类型：input textarea
    vModel: [String, Number],
    prepend: "", // 输入框前缀
    append: "", // 输入框后缀,
    rows: Number,
  },
};
</script>

