<template>
  <el-dialog title="说明" :visible.sync="visible" @close="DialogClose" width="500px">
 
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
