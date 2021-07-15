<template>
  <el-dialog title="说明" :visible.sync="visible" @close="DialogClose" width="500px">
 <div>
   <p>
     滑点0.01 手续费万分之2.5 印花税千分之1
   </p>
   <p>
     交易价格为每个交易日当天的收盘价
   </p>
   <p>
     初始资金为100万
   </p>
   <p>
     手续费包括券商手续费和印花税
   </p>
   <p>
     浮动盈亏已经扣除滑点的损失
   </p>
 </div>
  </el-dialog>
</template>

<style>
</style>

<script>
export default {
  name: "readmerDialog",
  data() {
    return {
      visible: false,
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
