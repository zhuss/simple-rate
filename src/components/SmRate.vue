<template>
  <div class="sm-rate">
    <div class="sm-rate-title">{{ options.title }}</div>
    <!-- 进度条 -->
    <div class="sm-rate-progress_group">
      <div
        class="sm-rate-progress_wrap"
        v-for="(item, index) in progress_list"
        :key="index"
      >
        <div
          class="sm-rate-progress_inner"
          :style="{ width: item + '%' }"
        ></div>
      </div>
    </div>
    <!-- 进度条 -->
    <!-- 指示器 -->
    <div
      class="sm-rate-indicator_group"
      v-if="options.indicator_list && options.indicator_list.length >= 2"
    >
      <div
        class="sm-rate-indicator-item"
        v-for="(item, index) in options.indicator_list"
        :key="index"
      >
        <div class="sm-indicator-name">
          <div>{{ item.name }}</div>
          <div class="sm-indicator-dot">
            <span></span>
          </div>
        </div>
      </div>
    </div>
    <!-- 指示器 -->
    <!-- 描述 -->
    <div
      class="sm-rate-description"
      v-if="currentRate && options.indicator_list.length >= 2"
    >
      {{ currentRate.description }}
    </div>
    <!-- 描述 -->
  </div>
</template>
<script>
export default {
  name: "SmRate",
  props: {
    current: {
      type: [Number, String],
      defulat: 0
    },
    options: {
      type: Object,
      default: () => {
        return {
          /** 参数模板 **/
          title: "指示器标题",
          split: 5,
          max: 20,
          indicator_list: [
            {
              rate: 0,
              name: "低",
              description: "低指标描述"
            },
            {
              rate: 10,
              name: "中",
              description: "中等指标描述"
            },
            {
              rate: 15,
              name: "高",
              description: "高指标描述"
            }
          ]
        };
      }
    }
  },
  computed: {
    progress_list() {
      let result = [];
      let current = this.current || 0;
      let max = this.options.max || 20;
      let split = this.options.split || 5;
      let step = max / split;
      for (let i = 0; i < split; i++) {
        if (current > step) {
          result.push(100);
        } else if (current < 0) {
          result.push(0);
        } else {
          result.push((current / step) * 100);
        }
        current -= step;
      }
      return result;
    },
    /*当前提示信息*/
    currentRate() {
      let current_indicator = null;
      let current = this.current || 0;
      for (let i = 0; i < this.options.indicator_list.length; i++) {
        if (current >= this.options.indicator_list[i].rate) {
          current_indicator = this.options.indicator_list[i];
        }
      }
      return current_indicator;
    }
  }
};
</script>
<style lang="less" scoped>
.sm-rate {
  background: #fff;
  padding: 26px;
  border-radius: 4px;
  > .sm-rate-title {
    padding-left: 14px;
    height: 22px;
    line-height: 22px;
    font-size: 18px;
    font-weight: 500;
    position: relative;
    &::before {
      display: block;
      content: "";
      position: absolute;
      top: 2px;
      left: 0;
      width: 4px;
      height: 18px;
      border-radius: 4px;
      background: #42b983;
    }
  }
  > .sm-rate-progress_group {
    margin-top: 20px;
    display: flex;
    > .sm-rate-progress_wrap {
      flex: 1;
      height: 12px;
      background: #f2f3f7;
      border-radius: 4px;
      overflow: hidden;
      & + .sm-rate-progress_wrap {
        margin-left: 8px;
      }
      > .sm-rate-progress_inner {
        height: 12px;
        background: #42b983;
      }
    }
  }
  > .sm-rate-indicator_group {
    margin-top: 10px;
    display: flex;
    > .sm-rate-indicator-item {
      flex: 1;
      text-align: center;
      &:first-child {
        text-align: left;
      }
      &:last-child {
        text-align: right;
      }
      > .sm-indicator-name {
        position: relative;
        color: #606266;
        display: inline-block;
        > .sm-indicator-dot {
          > span {
            display: block;
            width: 0;
            height: 0;
            margin: 0 auto;
            border-left: 8px solid transparent;
            border-right: 8px solid transparent;
            border-bottom: 8px solid #42b983;
          }
        }
      }
    }
  }
  > .sm-rate-description {
    margin-top: 20px;
    background: #f1f2f6;
    border-radius: 4px;
    padding: 10px;
    color: #909399;
    font-size: 14px;
  }
}
</style>
