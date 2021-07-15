<template>
  <el-dialog title="结算" :visible.sync="visible" @close="DialogClose" width="500px">
 <div>
  
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
