<template>
  <el-dialog title="交易" :visible.sync="visible" @close="DialogClose" width="500px">
    <div id="top-box">
      <div class="title">总资产:{{totalCapital}}</div>
      <el-row>
        <el-col :span="8">
          <div class="title2">持有市值:{{marketValue}}</div>
        </el-col>
        <el-col :span="8">
          <div class="title2">浮动盈亏:{{FPL}}</div>
        </el-col>
        <el-col :span="8">
          <div class="title2">可用资金:{{cash}}</div>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="8">
          <div class="title2">平均成本:{{avarageCost}}</div>
        </el-col>
        <el-col :span="8">
          <div class="title2">仓位:{{totalPosition}}%</div>
        </el-col>
        <el-col :span="8">
          <div class="title2">总手续费:{{totalFee}}</div>
        </el-col>
      </el-row>
    </div>
    <div>
      <el-tabs v-model="activeName" type="border-card" @tab-click="handleClick">
        <el-tab-pane label="买入" name="buy">
          <div class="trade">价格 : {{priceNow}}</div>
          <div class="trade">
            仓位 :
            <el-input-number
              v-model="position"
              :step="100"
              step-strictly
              size="small"
              :min="100"
              label="仓位"
            ></el-input-number>
            <label class="tip">最多可买{{maxPosition}}</label>
            <div class="ratio">
              <el-radio-group @change="changPos" v-model="posProp" size="mini">
                <el-radio-button label="0.25">1/4</el-radio-button>
                <el-radio-button label="0.5">1/2</el-radio-button>
                <el-radio-button label="0.75">3/4</el-radio-button>
                <el-radio-button label="1">全仓</el-radio-button>
              </el-radio-group>
              <el-button
                size="mini"
                type="primary"
                plain
                icon="el-icon-close"
                circle
                @click="canclePosProp"
              ></el-button>
            </div>
          </div>
          <div class="info">订单金额：{{orderAmount}} | 手续费：{{fee}} | 可用资金：{{cashLeft}}</div>
        </el-tab-pane>
        <el-tab-pane label="卖出" name="sell">
          <div class="trade">价格 : {{priceNow}}</div>
          <div class="trade">
            仓位 :
            <el-input-number
              v-model="position"
              :step="100"
              step-strictly
              size="small"
              :min="100"
              label="仓位"
            ></el-input-number>
            <label class="tip">最多可卖{{positionNow}}</label>
            <div class="ratio">
              <el-radio-group @change="changPos" v-model="posProp" size="mini">
                <el-radio-button label="0.25">1/4</el-radio-button>
                <el-radio-button label="0.5">1/2</el-radio-button>
                <el-radio-button label="0.75">3/4</el-radio-button>
                <el-radio-button label="1">全仓</el-radio-button>
              </el-radio-group>
              <el-button
                size="mini"
                type="primary"
                plain
                icon="el-icon-close"
                circle
                @click="canclePosProp"
              ></el-button>
            </div>
          </div>
          <div class="info">订单金额：{{orderAmount}} | 手续费：{{fee}} | 可用资金：{{cashLeft}}</div>
        </el-tab-pane>
      </el-tabs>
      <el-button style="margin-top:15px;" type="primary" @click="trade()">确定</el-button>
    </div>
  </el-dialog>
</template>

<style>
#top-box {
  border: 1px solid #dcdfe6;
  border-radius: 3px;
  padding: 7px;
  margin-bottom: 15px;
  box-shadow: 0 2px 4px 0 rgb(0 0 0 / 12%), 0 0 6px 0 rgb(0 0 0 / 4%);
}
.title {
  font-size: 24px;
  font-weight: bold;
  color: #606266;
  margin-bottom: 8px;
}
.title2 {
  font-size: 15px;
  color: #606266;
}
.trade {
  padding: 8px;
  font-size: 18px;
}
.tip {
  font-size: 14px;
}
.info {
  margin-top: 10px;
  font-size: 14px;
}
.ratio {
  margin-top: 5px;
}
</style>

<script>
export default {
  name: "tradeDialog",
  data() {
    return {
      visible: false,
      activeName: 'buy',
      initCapital: 1000000,
      totalCapital: 1000000,
      marketValue: 0,
      FPL: 0,
      lastFPL: 0,
      cash: 1000000,
      priceNow: null,
      positionNow: 0,
      position: null,
      posProp: null,
      totalPosition: 0,
      avarageCost: 0,
      totalFee: 0,
      maxPosition: null,
      orderAmount: '--',
      fee: '--',
      cashLeft: 1000000,
      slippage: 0.01,
      c_rate: 2.5 / 10000,
      t_rate: 1 / 1000
    };
  },
  methods: {
    init(price) {
      this.visible = true
      this.priceNow = parseFloat(price.toFixed(2))
      this.maxPosition = Math.floor(this.cash / ((this.priceNow + this.slippage) * (this.c_rate + 1) * 100)) * 100
      this.marketValue = parseFloat((this.positionNow * this.priceNow).toFixed(2))
      this.totalCapital = parseFloat(this.marketValue + this.cash).toFixed(2)
      this.FPL = parseFloat((this.totalCapital - this.initCapital).toFixed(2))
      if (this.positionNow) {
        this.avarageCost = this.floatFix2((this.marketValue - this.FPL + this.lastFPL) / this.positionNow)
      } {
        this.lastFPL = this.FPL
      }
      this.posProp = null
      this.calcTotalPosition()
    },
    reset() {
      this.totalCapital = 1000000
      this.marketValue = 0
      this.FPL = 0
      this.lastFPL = 0
      this.cash = 1000000
      this.priceNow = null
      this.positionNow = 0
      this.position = null
      this.posProp = null
      this.maxPosition = null
      this.totalPosition = 0
      this.avarageCost = 0
      this.totalFee = 0
      this.orderAmount = '--'
      this.fee = '--'
      this.cashLeft = 1000000
    },
    DialogClose() {
      this.visible = false
    },
    handleClick() {
      this.position = null
      this.posProp = null
    },
    trade() {
      if (this.activeName == 'buy') {
        if (this.cashLeft > 0) {
          this.positionNow += this.position
          this.marketValue += this.orderAmount
          const loss = this.fee + this.slippage * this.position
          this.FPL -= loss
          this.totalFee = this.floatFix2(this.totalFee + this.fee)
          this.totalCapital = parseFloat((this.totalCapital - this.fee - loss).toFixed(2))
          this.cash = this.cashLeft
          this.position = null
          this.calcTotalPosition()
          this.$message({
            message: '交易成功',
            type: 'success',
            duration: 1500,
            onClose: () => {
              this.visible = false
            }
          })
        } else {
          this.$message.error('可用资金不足')
        }
      } else if (this.activeName == 'sell') {
        if (this.position <= this.positionNow) {
          this.positionNow -= this.position
          this.marketValue -= this.positionNow * this.priceNow
          const loss = this.fee + this.slippage * this.position
          this.FPL -= loss
          this.totalFee = this.floatFix2(this.totalFee + this.fee)
          this.totalCapital = parseFloat((this.totalCapital - this.fee - loss).toFixed(2))
          this.cash = this.cashLeft
          this.position = null
          this.calcTotalPosition()
          this.$message({
            message: '交易成功',
            type: 'success',
            duration: 1500,
            onClose: () => {
              this.visible = false
            }
          })
        } else {
          this.$message.error('可用持仓数量不足')
        }
      }
    },
    changPos(pos) {
      if (this.activeName == 'buy') {
        this.position = Math.floor(this.maxPosition * parseFloat(pos))
      } else if (this.activeName == 'sell') {
        this.position = Math.floor(this.positionNow * parseFloat(pos))
      }
    },
    canclePosProp() {
      this.posProp = null
      this.position = 100
    },
    calcProfitRate() {
      return ((this.totalCapital / this.initCapital - 1) * 100).toFixed(1)
    },
    calcTotalPosition() {
      this.totalPosition = (this.marketValue * 100 / this.totalCapital).toFixed(1)
    },
    floatFix2(num) {
      return parseFloat(num.toFixed(2))
    },
    sendProfitRate(price) {
      this.priceNow = price
      this.marketValue = parseFloat((this.positionNow * this.priceNow).toFixed(2))
      this.totalCapital = parseFloat(this.marketValue + this.cash).toFixed(2)
      this.$emit('getrateData', this.calcProfitRate())
    }
  },

  watch: {
    position(pos) {
      if (this.activeName == 'buy') {
        // 订单金额
        this.orderAmount = parseFloat(((this.priceNow + this.slippage) * pos).toFixed(2))
        // 手续费
        var f = parseFloat((this.orderAmount * this.c_rate).toFixed(2))
        if (f < 5) {
          this.fee = 5
        } else {
          this.fee = f
        }
        // 可用资金-买入
        this.cashLeft = parseFloat((this.cash - this.fee - this.orderAmount).toFixed(2))
      } else {
        // 订单金额
        this.orderAmount = parseFloat(((this.priceNow - this.slippage) * pos).toFixed(2))
        // 手续费
        var f = parseFloat((this.orderAmount * this.c_rate).toFixed(2))
        var t = parseFloat((this.orderAmount * this.t_rate).toFixed(2))
        if (f < 5) {
          this.fee = 5 + t
        } else {
          this.fee = f + t
        }
        this.fee = parseFloat(this.fee.toFixed(2))
        // 可用资金-买入
        this.cashLeft = parseFloat((this.cash - this.fee + this.orderAmount).toFixed(2))
      }
    }
  }
};
</script>
