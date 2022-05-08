
<template>
  <el-tabs editable @edit="handleTabsEdit" type="card">
    <el-tab-pane :key="item.name" v-for="item in editableTabs" :label="item.name">
      {{ item.content }}
    </el-tab-pane>
  </el-tabs>
</template>
<script>
import Sortable from 'sortablejs'
export default {
  data() {
    return {
      editableTabs: [
        {
          name: '1',
          content: 'Tab 1 content'
        },
        {
          name: '2',
          content: 'Tab 2 content'
        },
        {
          name: '3',
          content: 'Tab 3 content'
        },
        {
          name: '4',
          content: 'Tab 4 content'
        },
      ],
    }
  },
  mounted() {
    setTimeout(this.rowDrop)
  },
  methods: {
    handleTabsEdit(targetName, action) {
      if (action === 'add') {
        let newTabName = ++this.tabIndex + '';
        this.editableTabs.push({
          name: Date.now() + '',
          content: `New Tab content ${this.editableTabs.length + 1}`
        });
        this.editableTabsValue = newTabName;
      }
      if (action === 'remove') {
        let tabs = this.editableTabs;
        let activeName = this.editableTabsValue;
        if (activeName === targetName) {
          tabs.forEach((tab, index) => {
            if (tab.name === targetName) {
              let nextTab = tabs[index + 1] || tabs[index - 1];
              if (nextTab) {
                activeName = nextTab.name;
              }
            }
          });
        }

        this.editableTabsValue = activeName;
        this.editableTabs = tabs.filter(tab => tab.name !== targetName);
      }
    },
    rowDrop() {
      const _this = this;
      Sortable.create(document.querySelector(".el-tabs__nav"), {
        onEnd(evt) {
          const oldItem = _this.editableTabs[evt.oldIndex]
          _this.editableTabs.splice(evt.oldIndex, 1)
          _this.editableTabs.splice(evt.newIndex, 0, oldItem)
        },
      });
    },
  }
}
</script>