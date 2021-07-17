<template>
  <div id="app">
    <header>
      <el-row>
        <el-button size="small" type="primary" @click="KlineDialogHandle()">获取K线</el-button>
        <el-button size="small" type="success" @click="tradeDialogHandle()">交易</el-button>
        <el-button size="small" type="info" @click="passOne()">观望</el-button>
        <el-button size="small" type="warning" @click="settleDialogHandle()">结算</el-button>
        <el-button size="small" type="info" plain class="right" @click="readmeDialogHandle()">说明</el-button>
      </el-row>
    </header>
    <main id="main">
      <Vue-kline :klineParams="klineParams" :klineData="klineData"></Vue-kline>
    </main>
    <!-- 获得K线 dialog -->
    <get-Kline-Dialog ref="getKlineDialog" v-if="KlineDialogVisible" @getData="handleRawData"></get-Kline-Dialog>
    <!-- 交易 dialog -->
    <trade-Dialog @getrateData="handleRateData" ref="tradeDialog" v-if="tradeDialogVisible"></trade-Dialog>
    <readme-Dialog ref="readmeDialog" v-if="readmeDialogVisible"></readme-Dialog>
    <settle-Dialog ref="settleDialog" v-if="settleDialogVisible"></settle-Dialog>
  </div>
</template>

<style>
html,
body {
  height: 100%;
}
/* #app {
  height: 100%;
} */
main {
  height: 100%;
}
header {
  padding: 10px;
  background-color: #f6f6f6;
}
.right {
  float: right;
}
</style>

<script>
import VueKline from "./kline/kline";
import { Chart } from './kline/js/chart'
import getKlineDialog from "./game/getKlineDialog"
import tradeDialog from "./game/tradeDialog"
import readmeDialog from "./game/readmeDialog"
import settleDialog from "./game/settleDialog"
export default {
  name: "stock-game",
  components: {
    VueKline,
    getKlineDialog,
    tradeDialog,
    readmeDialog,
    settleDialog
  },
  data() {
    return {
      meg: "first vue-cli test, welcome you coming",
      klineParams: {
        width: 1200,
        height: 600,
        theme: "light",
        language: "zh-cn",
        ranges: ["1w", "1d", "line"],
        symbol: "",
        symbolName: "",
        intervalTime: 50000,
        depthWidth: 50,
        count: 1
      },
      klineData: {
        success: true,
        data: {
          lines: [
            [0, 1.11, 1.11, 1.11, 1.11, 0],
          ]        }
      },
      KlineDialogVisible: false,
      tradeDialogVisible: false,
      readmeDialogVisible: false,
      settleDialogVisible: false,
      rawData: {},
      newList: [],
      position: 250,
      priceStart: null,
      priceNow: null,
      tradeCount: 0,
      profitRate: 0,
    };
  },
  created() {
    const height = document.body.clientHeight
    const width = document.body.clientWidth
    this.klineParams.height = height - 60
    this.klineParams.width = width - 2
  },
  methods: {
    handleRawData(data) {
      this.rawData = data
      this.position = 250
      console.log(this.rawData)
      if (this.rawData.error_code == 0) {
        this.newList = this.rawData.data.item.map(function (item) {
          var newItem = item.slice(0, 6)
          var volume = parseFloat((newItem[1] / 1000000).toFixed(2))
          newItem.splice(1, 1)
          newItem.push(volume)
          return newItem
        })
        if (this.newList.length > 700) {
          this.position = Math.round(Math.random() * (300) + 200)
          this.klineData.data.lines = this.newList.slice(0, this.position + 1)
        } else if (700 > this.newList.length > 400) {
          this.position = Math.round(Math.random() * (this.newList.length - 400) + 200)
          this.klineData.data.lines = this.newList.slice(0, this.position + 1)
        } else if (this.newList.length > this.position) {
          this.klineData.data.lines = this.newList.slice(0, this.position + 1)
        } else {
          this.klineData.data.lines = this.newList
        }
        this.updateDisplay()
        // 重置交易信息
        this.$nextTick(() => {
          this.$refs.tradeDialog.reset()
        })
        const arr = this.klineData.data.lines
        this.priceStart = arr[arr.length - 1][4]
        this.tradeCount = 0
      } else {
        this.$message.error('数据有问题')
      }
    },
    handleRateData(data) {
      this.profitRate = data
    },
    // dialog handle
    KlineDialogHandle() {
      this.KlineDialogVisible = true
      this.$nextTick(() => {
        this.$refs.getKlineDialog.init()
      })
    },
    tradeDialogHandle() {
      this.tradeDialogVisible = true
      this.$nextTick(() => {
        this.$refs.tradeDialog.init(this.priceNow)
      })
    },
    readmeDialogHandle() {
      this.readmeDialogVisible = true
      this.$nextTick(() => {
        this.$refs.readmeDialog.init()
      })
    },
    settleDialogHandle() {
      this.settleDialogVisible = true
      const change = ((this.priceNow / this.priceStart - 1) * 100).toFixed(1)
      this.$nextTick(() => {
        this.$refs.tradeDialog.sendProfitRate(this.priceNow)
      })
      this.$nextTick(() => {
        this.$refs.settleDialog.init(this.tradeCount, change, this.profitRate)
      })
    },
    // 
    passOne() {
      if (this.newList.length > this.position) {
        this.position += 1
        this.tradeCount += 1
        this.klineData.data.lines = this.newList.slice(0, this.position + 1)
        this.updateDisplay()
      } else {
        this.$message.error('K线已到尽头')
      }
    },
    updateDisplay() {
      const newData = this.klineData.data.lines
      var chart = new Chart()
      chart.updateDataAndDisplay(newData)
      // chart.updateDataAndDisplay(newData) //不优雅的解决一个vue-kline自带的bug
      this.getNewPrice()
    },
    getNewPrice() {
      const arr = this.klineData.data.lines
      this.priceNow = arr[arr.length - 1][4]
      // console.log(this.priceNow)
    }
  }
}
</script>