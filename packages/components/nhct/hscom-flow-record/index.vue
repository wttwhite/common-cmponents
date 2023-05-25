<template>
  <div class="flow-record">
    <el-timeline>
      <el-timeline-item
        v-for="(item, index) in activities"
        :key="index"
        :icon="item.icon"
        :type="item.type"
        :color="index == 0 ? '#0bbd87' : ''"
        :size="item.size"
        :timestamp="item.finishTime || '--'"
        placement="top"
      >
        <div class="list">
          <span v-if="item.taskName">节点名称：{{ item.taskName }} </span>
          <span v-if="item.assigneeName">
            处理人：{{ item.assigneeName }}
          </span>
          <span v-if="item.comment && item.comment.type">
            处理结果：{{ commitType[item.comment.type] }}
          </span>
          <span v-if="item.comment">
            处理意见： {{ item.comment.comment }}
          </span>
        </div>
      </el-timeline-item>
    </el-timeline>
  </div>
</template>

<script>
export default {
  name: "hscom-flow-record",
  props: {
    finish: {
      type: Boolean,
      default: true,
    },
    list: {
      type: Array,
      default: () => [],
    },
  },
  components: {},
  data() {
    return {
      // CXTJ  提交（发起流程）
      // TJ  提交（发起流程）
      // SP  审批（审批通过）
      // TH  退回（审批被驳回）
      // ZF  作废（发起人作废）
      // WC  完成（完整流程通过）
      // CH  撤回（发起人撤回）
      // TG  自动跳过（尽量不要用，貌似有跳过的流程获取form数据会有部分表单数据丢失情况）
      commitType: {
        CXTJ: "重新提交",
        TJ: "提交",
        SP: "同意",
        TH: "驳回",
        ZF: "作废",
        CH: "撤回",
        TG: "跳过",
      },
      activities: [
        {
          content: "支持使用图标",
          timestamp: "2018-04-12 20:46",
          type: "primary",
          color: "#0bbd87",
          icon: "el-icon-more",
          name: "sc",
          dept: "研发部",
          nodeType: `<span style="color:red">操作类型：审批通过</span>`,
        },
        {
          content: "支持自定义颜色",
          timestamp: "2018-04-03 20:46",
          color: "#0bbd87",
          name: "sc",
          dept: "研发部",
        },
        {
          content: "支持自定义尺寸",
          timestamp: "2018-04-03 20:46",
          name: "sc",
          dept: "研发部",
        },
        {
          content: "默认样式的节点",
          timestamp: "2018-04-03 20:46",
          name: "sc",
          dept: "研发部",
        },
      ],
    };
  },
  computed: {},
  watch: {
    list: {
      handler(val) {
        this.activities = JSON.parse(JSON.stringify(val)).reverse();
      },
      deep: true,
      immediate: true,
    },
  },
};
</script>
<style lang="scss" scoped>
.flow-record {
  width: 100%;
  height: 100%;
  display: flex;
  position: relative;
  justify-content: flex-start;
  // padding: 16px;
  padding-left: 20%;
}
.list {
  display: flex;
  flex-direction: column;
  span {
    padding: 4px 0;
  }
}
</style>
