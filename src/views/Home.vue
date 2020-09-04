<template>
  <div class="home">
    <h2 class="title">simple-rate</h2>
    <div class="subtitle">简单指示器组件</div>
    <div class="input-wrap">
      <span>当前值：</span>
      <input type="text" v-model.number="current" />
    </div>
    <div class="demo-block">
      <sm-rate :current="current" :options="options"></sm-rate>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import SmRate from "../components/SmRate";
export default {
  name: "home",
  components: {
    SmRate
  },
  data() {
    return {
      current: 10,
      options: {
        /** 参数模板 **/
        title: "指示器标题",
        split: 5,
        max: 100,
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
      }
    };
  }
};
</script>
<style lang="less" scoped>
.title,
.subtitle {
  text-align: center;
}
.input-wrap {
  margin-bottom: 20px;
  text-align: center;
}
.demo-block {
  width: 800px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #e6e6e6;
  background: #f8f8f8;
}
</style>
