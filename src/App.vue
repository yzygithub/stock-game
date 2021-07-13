<template>
  <div id="app">
    <header>
      <el-row>
        <el-button size="small" type="primary" @click="KlineDialogHandle()">获取K线</el-button>
        <el-button size="small" type="success">交易</el-button>
        <el-button size="small" type="info">观望</el-button>
        <el-button size="small" type="warning">结算</el-button>
      </el-row>
    </header>
    <main id="main">
      <Vue-kline :klineParams="klineParams" :klineData="klineData"></Vue-kline>
    </main>
    <!-- 获得K线 -->
    <get-Kline-Dialog ref="getKlineDialog" v-if="KlineDialogVisible" @getData="rawDataHandle"></get-Kline-Dialog>
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
</style>

<script>
import VueKline from "./kline/kline";
import { Chart } from './kline/js/chart'
import getKlineDialog from "./game/getKline"
export default {
  name: "stock-game",
  components: {
    VueKline,
    getKlineDialog
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
      klineData: {},
      KlineDialogVisible: false,
      rawData: {
        "data": {
          "symbol": "SH000001",
          "column": [
            "timestamp",
            "volume",
            "open",
            "high",
            "low",
            "close",
            "chg",
            "percent",
            "turnoverrate",
            "amount",
            "volume_post",
            "amount_post"
          ],
          "item": [
            [
              1558627200000,
              16720549000,
              2847.8356,
              2871.8555,
              2846.0215,
              2852.99,
              0.47,
              0.02,
              0.62,
              162689316181.4,
              null,
              null
            ],
            [
              1558886400000,
              19672090200,
              2851.2809,
              2898.129,
              2833.0363,
              2892.38,
              39.39,
              1.38,
              0.73,
              193768714251.8,
              null,
              null
            ],
            [
              1558972800000,
              22331055700,
              2890.2658,
              2924.0395,
              2887.0835,
              2909.91,
              17.53,
              0.61,
              0.86,
              227253813690,
              null,
              null
            ],
            [
              1559059200000,
              19895882100,
              2894.8297,
              2934.9768,
              2890.6645,
              2914.7,
              4.79,
              0.16,
              0.76,
              202855039152.7,
              null,
              null
            ],
            [
              1559145600000,
              20575878900,
              2903.4246,
              2907.8463,
              2881.3802,
              2905.81,
              -8.89,
              -0.31,
              0.74,
              197105555808.7,
              null,
              null
            ],
            [
              1559232000000,
              19523058000,
              2904.4976,
              2922.9075,
              2895.5778,
              2898.7,
              -7.11,
              -0.24,
              0.72,
              191280403618.3,
              null,
              null
            ],
            [
              1559491200000,
              21594355400,
              2901.7424,
              2920.8292,
              2875.9019,
              2890.08,
              -8.62,
              -0.3,
              0.79,
              208073320323.1,
              null,
              null
            ],
            [
              1559577600000,
              18846354100,
              2887.6405,
              2888.3861,
              2851.9728,
              2862.28,
              -27.8,
              -0.96,
              0.69,
              181118915530.5,
              null,
              null
            ],
            [
              1559664000000,
              18156104700,
              2882.9369,
              2888.7676,
              2858.5719,
              2861.42,
              -0.86,
              -0.03,
              0.67,
              175200844236.4,
              null,
              null
            ],
            [
              1559750400000,
              17706441100,
              2862.3327,
              2862.3327,
              2822.1853,
              2827.8,
              -33.62,
              -1.17,
              0.64,
              164850546682.3,
              null,
              null
            ],
            [
              1560096000000,
              16658591800,
              2833.0145,
              2861.131,
              2824.3554,
              2852.13,
              24.33,
              0.86,
              0.62,
              162489831383.9,
              null,
              null
            ],
            [
              1560182400000,
              26052579600,
              2854.0704,
              2927.4303,
              2854.0704,
              2925.72,
              73.59,
              2.58,
              0.95,
              254479985773.8,
              null,
              null
            ],
            [
              1560268800000,
              24025577500,
              2917.2239,
              2924.7018,
              2903.8833,
              2909.38,
              -16.34,
              -0.56,
              0.84,
              223063953327.3,
              null,
              null
            ],
            [
              1560355200000,
              21295779100,
              2905.2877,
              2918.4227,
              2885.9232,
              2910.74,
              1.36,
              0.05,
              0.72,
              191748206694.5,
              null,
              null
            ],
            [
              1560441600000,
              21543579600,
              2912.9988,
              2924.3197,
              2879.6573,
              2881.97,
              -28.77,
              -0.99,
              0.74,
              194303111838,
              null,
              null
            ],
            [
              1560700800000,
              15396496000,
              2880.4226,
              2902.4808,
              2877.3941,
              2887.62,
              5.65,
              0.2,
              0.54,
              143141637323.7,
              null,
              null
            ],
            [
              1560787200000,
              14770532000,
              2891.0917,
              2898.3304,
              2874.3129,
              2890.16,
              2.54,
              0.09,
              0.52,
              136935231690.1,
              null,
              null
            ],
            [
              1560873600000,
              23075697300,
              2944.1154,
              2953.3355,
              2916.2149,
              2917.8,
              27.64,
              0.96,
              0.85,
              225567257985.6,
              null,
              null
            ],
            [
              1560960000000,
              29101153700,
              2917.331,
              2997.3888,
              2915.0895,
              2987.12,
              69.317,
              2.38,
              1.06,
              288296546353.3,
              null,
              null
            ],
            [
              1561046400000,
              28697383500,
              2990.3687,
              3010.349,
              2989.2454,
              3001.98,
              14.861,
              0.5,
              1.01,
              276662458531.2,
              null,
              null
            ],
            [
              1561305600000,
              21049389400,
              3004.2891,
              3012.826,
              2994.4219,
              3008.15,
              6.17,
              0.21,
              0.77,
              211237183001.3,
              null,
              null
            ],
            [
              1561392000000,
              24369924400,
              3004.9095,
              3004.9095,
              2948.9907,
              2982.07,
              -26.078,
              -0.87,
              0.9,
              244315077409.3,
              null,
              null
            ],
            [
              1561478400000,
              17444953300,
              2964.6153,
              2986.9139,
              2958.4632,
              2976.28,
              -5.79,
              -0.19,
              0.63,
              172516055226.7,
              null,
              null
            ],
            [
              1561564800000,
              19539704900,
              2982.6125,
              3011.538,
              2981.045,
              2996.79,
              20.506,
              0.69,
              0.76,
              208618648070.5,
              null,
              null
            ],
            [
              1561651200000,
              18143152300,
              2992.2426,
              2992.2426,
              2961.2225,
              2978.88,
              -17.913,
              -0.6,
              0.67,
              183641767342.2,
              null,
              null
            ],
            [
              1561910400000,
              25084043300,
              3024.6238,
              3045.3669,
              3014.6871,
              3044.9,
              66.022,
              2.22,
              0.96,
              266541056906.7,
              null,
              null
            ],
            [
              1561996800000,
              21452062400,
              3042.58,
              3048.4839,
              3033.7809,
              3043.94,
              -0.96,
              -0.03,
              0.83,
              229566616522.2,
              null,
              null
            ],
            [
              1562083200000,
              21229617300,
              3031.832,
              3031.832,
              3006.3246,
              3015.26,
              -28.68,
              -0.94,
              0.79,
              218657994390.2,
              null,
              null
            ],
            [
              1562169600000,
              19441513300,
              3015.6785,
              3024.5023,
              2991.9079,
              3005.25,
              -10.013,
              -0.33,
              0.74,
              202266388594.3,
              null,
              null
            ],
            [
              1562256000000,
              15675542900,
              3004.7383,
              3014.8474,
              2990.9247,
              3011.06,
              5.81,
              0.19,
              0.59,
              163180842756.1,
              null,
              null
            ],
            [
              1562515200000,
              21154126800,
              2997.8067,
              2997.8067,
              2918.7157,
              2933.36,
              -77.7,
              -2.58,
              0.77,
              205568813499.7,
              null,
              null
            ],
            [
              1562601600000,
              14511060400,
              2928.8188,
              2937.7621,
              2912.8063,
              2928.23,
              -5.133,
              -0.17,
              0.58,
              154251421843.3,
              null,
              null
            ],
            [
              1562688000000,
              13548353800,
              2935.1855,
              2936.7519,
              2908.162,
              2915.3,
              -12.929,
              -0.44,
              0.53,
              141236258614,
              null,
              null
            ],
            [
              1562774400000,
              14369252200,
              2928.0553,
              2945.8023,
              2907.8852,
              2917.76,
              2.458,
              0.08,
              0.57,
              152181407950.6,
              null,
              null
            ],
            [
              1562860800000,
              13779475900,
              2915.3401,
              2938.5311,
              2905.809,
              2930.55,
              12.789,
              0.44,
              0.53,
              142918869589.5,
              null,
              null
            ],
            [
              1563120000000,
              16764339700,
              2921.55,
              2955,
              2886.7,
              2942.19,
              11.64,
              0.4,
              0.66,
              176533201236.5,
              null,
              null
            ],
            [
              1563206400000,
              13181175100,
              2938.6411,
              2944.821,
              2931.2926,
              2937.62,
              -4.565,
              -0.16,
              0.52,
              140766389027.7,
              null,
              null
            ],
            [
              1563292800000,
              14963161600,
              2933.0227,
              2942.0668,
              2924.4594,
              2931.69,
              -5.927,
              -0.2,
              0.57,
              152162962633,
              null,
              null
            ],
            [
              1563379200000,
              14988882800,
              2921.7387,
              2921.7387,
              2901.18,
              2901.18,
              -30.513,
              -1.04,
              0.56,
              149088843109.1,
              null,
              null
            ],
            [
              1563465600000,
              14946955300,
              2909.6794,
              2939.5973,
              2909.6794,
              2924.2,
              23.023,
              0.79,
              0.56,
              149071212018.7,
              null,
              null
            ],]
        },
        "error_code": 0,
        "error_description": ""
      },
    };
  },
  created() {
    const height = document.body.clientHeight
    const width = document.body.clientWidth
    this.klineParams.height = height - 60
    this.klineParams.width = width - 2
    this.klineParams.symbolName = this.rawData.data.symbol
    this.handleRawData()
  },
  mounted() {
  },
  methods: {
    handleRawData() {
      if (this.rawData.error_code == 0) {
        this.klineParams.symbol = this.rawData.data.symbol
        this.klineParams.symbolName = this.rawData.data.symbol
        var newList = this.rawData.data.item.map(function (item) {
          var newItem = item.slice(0, 6)
          var volume = parseFloat((newItem[1] / 1000000).toFixed(2))
          newItem.splice(1, 1)
          newItem.push(volume)
          return newItem
        })
        this.klineData = {
          success: true,
          data: {
            lines: newList
          }
        }
        console.log(this.klineData)
        const newData = this.klineData.data.lines
        var chart = new Chart()
        chart.updateDataAndDisplay(newData)
        // chart.setSymbol(this.klineParams.symbol);
      } else {
        this.$message.error('数据有问题')
      }
    },
    KlineDialogHandle() {
      this.KlineDialogVisible = true
      this.$nextTick(() => {
        this.$refs.getKlineDialog.init()
      })
    },
    rawDataHandle(data) {
      // console.log(data)
      this.rawData = data
      console.log(this.rawData)
      this.handleRawData()
    }
  }
}
</script>