<template>
  <el-dialog title="交易" :visible.sync="visible" @close="DialogClose" width="450px">
   
  </el-dialog>
</template>

<script>
export default {
  name: "tradeDialog",
  data() {
    return {
      visible: false
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
