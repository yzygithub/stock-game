<template>
  <el-dialog title="结算" :visible.sync="visible" @close="DialogClose" width="500px">
    <div>
      {{tradeCount}}个交易周期，股票上涨了{{stockChange}}%,你的收益率{{profitRate}}%
    </div>
  </el-dialog>
</template>

<style>
</style>

<script>
export default {
  name: "settleDialog",
  data() {
    return {
      visible: false,
      tradeCount: null,
      stockChange:null,
      profitRate:null
    };
  },
  methods: {
    init(tradeCount,stockChange,profitRate) {
      this.visible = true
      this.tradeCount = tradeCount
      this.stockChange = stockChange
      this.profitRate = profitRate
    },
    DialogClose() {
      this.visible = false
    },
  },
};
</script>
