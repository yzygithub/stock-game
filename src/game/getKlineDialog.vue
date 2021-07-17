<template>
  <el-dialog title="获取K线" :visible.sync="visible" @close="DialogClose" width="450px">
    <el-alert
      title="因为无法解决跨域问题，只能d打开新页面访问接口，获得数据 保存为json文件，再读取本地文件获得数据"
      type="success"
      :closable="false"
      style="margin-bottom:10px;"
    ></el-alert>
    <el-form ref="form" :model="form" :rules="dataRule" label-width="80px" id="klineForm">
      <el-form-item size="small" label="股票代码" prop="symbol">
        <el-input v-model="form.symbol" maxlength="8" show-word-limit placeholder="字母需大写" style="width:160px;"></el-input>
        <el-button size="mini" @click="randomSymbol()">随机</el-button>
      </el-form-item>
      <el-form-item size="small" label="最后时间" prop="begin">
        <el-date-picker
          type="date"
          placeholder="选择日期"
          v-model="form.begin"
          value-format="timestamp"
        ></el-date-picker>
      </el-form-item>
      <el-form-item label="复权类型">
        <el-radio-group size="small" v-model="form.type">
          <el-radio-button label="before">前复权</el-radio-button>
          <el-radio-button label="after">后复权</el-radio-button>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="选择周期">
        <el-radio-group size="small" v-model="form.period">
          <el-radio-button label="day">日</el-radio-button>
          <el-radio-button label="week">周</el-radio-button>
          <el-radio-button label="month">月</el-radio-button>
        </el-radio-group>
      </el-form-item>
      <el-form-item size="small" label="周期数量">
        <el-input-number v-model="form.count" :min="100" :max="2500"></el-input-number>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="getData()">提交</el-button>
        <el-button @click="DialogClose()">取消</el-button>
      </el-form-item>
    </el-form>
    <el-divider content-position="left"></el-divider>
    <el-upload
      class="upload-demo"
      accept=".json"
      :on-change="changeFile"
      :auto-upload="false"
      action
    >
      <el-button size="small" type="primary">点击读取</el-button>
      <div slot="tip" class="el-upload__tip">读取已下载的json文件，只能读取json文件</div>
    </el-upload>
  </el-dialog>
</template>
<style>
.el-form-item {
  margin-bottom: 10px !important;
}
</style>

<script>
import symbols from '../symbols'
export default {
  name: "getKlineDialog",
  data() {
    return {
      form: {
        symbol: 'SH000001',
        begin: 1625932800000,
        type: 'before',
        count: 1000,
        period: 'day',
      },
      dataRule: {
        symbol: [
          { required: true, message: '代码不能为空', trigger: 'blur' },
          { min: 8, max: 8, message: '长度为8个字符', trigger: 'blur' }
        ],
        begin: [
          { required: true, message: '日期不能为空', trigger: 'blur' }
        ]
      },
      visible: false
    };
  },
  methods: {
    init() {
      this.visible = true
      var date = new Date()
      this.form.begin = date.getTime()
      // console.log(symbols)
    },
    DialogClose() {
      this.visible = false
    },
    getData() {
      this.$refs['form'].validate((valid) => {
        if (valid) {
          window.open(`https://stock.xueqiu.com/v5/stock/chart/kline.json?symbol=${this.form.symbol}&begin=${this.form.begin}&period=${this.form.period}&type=${this.form.type}&count=-${this.form.count}&indicator=kline`);
          console.log(this.form)
        }
      })
    },
    changeFile(file) {
      var reader = new FileReader()
      reader.onload = async (e) => {
        try {
          let document = JSON.parse(e.target.result)
          console.log(document)
          if (document.error_code == 0) {
            this.$message({
              message: '读取成功',
              type: 'success',
              duration: 800,
              onClose: () => {
                this.visible = false
                this.$emit('getData', document)
              }
            })
          } else {
            this.$message.error('数据有问题')
          }
        } catch (err) {
          console.log(`load JSON document from file error: ${err.message}`)
          this.showSnackbar(`Load JSON document from file error: ${err.message}`, 4000)
        }
      }
      reader.readAsText(file.raw)
    },
    randomSymbol() {
      const random = Math.random()
      const index = Math.round((symbols.length-1)*random)
      // console.log(symbols[index][0])
      this.form.symbol = symbols[index]
    }
  },
};
</script>
