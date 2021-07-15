<template>
  <el-dialog title="交易" :visible.sync="visible" @close="DialogClose" width="500px">
    <div>
      <div class="title">
        总资产:{{totalCapital}}
      </div>
      <el-row>
        <el-col :span="8">
          <div class="title2">
            持有市值:{{marketValue}}
          </div>
        </el-col>
        <el-col :span="8">
          <div class="title2">浮动盈亏:{{FPL}}</div>
        </el-col>
        <el-col :span="8">
          <div class="title2">可用资金:{{cash}}</div>
        </el-col>
      </el-row>
    </div>
    <el-divider content-position="left"></el-divider>
    <div>

    </div>
  </el-dialog>
</template>

<style>
.title {
  font-size: 24px;
  font-weight: bold;
  color: #333333;
  margin-bottom: 8px;
}
.title2 {
  font-size: 18px;
  color: #333333;
}
</style>

<script>
export default {
  name: "tradeDialog",
  data() {
    return {
      visible: false,
      totalCapital:1000000,
      marketValue:10000,
      FPL: 1000,
      cash:22222
    };
  },
  methods: {
    init() {
      this.visible = true
    },
    DialogClose() {
      this.visible = false
    },
  },
};
</script>
