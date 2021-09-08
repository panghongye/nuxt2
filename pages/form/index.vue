<template>
  <el-form ref="form" :model="config.model" class="form" label-width="100px">
    <p class="p-name">{{ config.title }}</p>
    <el-form-item
      v-for="item in config.list"
      :key="item.key"
      :label="item.label"
      :prop="item.key"
      :rules="item.rules"
    >
      <el-input
        v-if="item.type === 'input' || item.type === 'textarea'"
        v-model="config.model[item.key]"
        :rows="item.rows || 5"
        :type="item.type"
      >
        <template v-if="item.prepend" slot="prepend">{{
          item.prepend
        }}</template>
        <template v-if="item.append" slot="append">{{ item.append }}</template>
      </el-input>

      <el-radio-group
        v-if="item.type === 'radio'"
        v-model="config.model[item.key]"
      >
        <el-radio
          v-for="(radio, index) in item.list"
          :key="index"
          :label="radio.value"
        >
          {{ radio.label }}
        </el-radio>
      </el-radio-group>
    </el-form-item>
  </el-form>
</template>

<script>
export default {
  name: "form",
  props: {
    // 配置格式示例及说明
    config: {
      type: Object,
      default: () => ({
        title: "表单标题",
        model: {}, // 表单数据
        // 表单项列表
        list: [
          {
            type: "input", // 控件类型：input textarea
            key: "title", // 键名
            label: "邮箱", // 控件显示标题
            prepend: "", // 输入框前缀
            append: "", // 输入框后缀
            rules: [
              { required: true, message: "请输入邮箱地址", trigger: "blur" },
              {
                type: "email",
                message: "请输入正确的邮箱地址",
                trigger: ["blur", "change"],
              },
            ],
          },
          {
            type: "radio",
            key: "radio",
            label: "单选", // 控件标题
            rules: [{ required: true, message: "强选择", trigger: "blur" }],
            list: [
              { type: "value", value: 0, label: "a" },
              { type: "value", value: 1, label: "b" },
            ],
          },
        ],
      }),
    },
  },
  components: {},
  created() {
    global.t = this;
  },
};
</script>


<style lang='scss' scoped>
.p-name {
  font-weight: bold;
  border-bottom: 1px solid black;
  padding-bottom: 12px;
}
</style>
