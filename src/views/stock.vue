<template>
  <div>
    <nav-menu></nav-menu>
    <div class="stock-container">
      <div class="stock">
        <h2 style="margin: 15px 0">{{ stockInfo.stockName }} ({{ stockInfo.stockCode }})</h2>
        <vue-kline :klineParams="klineParams" :klineData="klineData" ref="callMethods" @refreshKlineData="refreshKlineData"></vue-kline>
      </div>
      <div class="user-info">
        <el-divider content-position="left"><span style="font-size: 18px; font-weight: bold">操作</span></el-divider>
        <div class="info">
          <span style="font-weight: bold">账户信息 </span>
          <span>余额: {{ tradeInfo.balance }}</span>
          <span>持股数量: {{ tradeInfo.tradeInfo[0].quantity }}</span>
          <span>持仓市值: {{ tradeInfo.tradeInfo[0].value }}</span>
          <span>持仓盈亏: {{ tradeInfo.tradeInfo[0].profit }}</span>
        </div>
      </div>
      <div class="trade">
        <span style="font-weight: bold">股票交易</span>
        <div>
<!--          <span style="font-weight: bold; font-size: 14px; margin-right: 15px">买入股票</span>-->
          <el-input-number v-model="buyNum" :min="1" size="small"></el-input-number>
          <el-button type="primary" size="small" plain @click="buy()">买入股票</el-button>
        </div>
        <div>
<!--          <span style="font-weight: bold; font-size: 14px; margin-right: 15px">卖出股票</span>-->
          <el-input-number v-model="sellNum" :min="1" size="small"></el-input-number>
          <el-button type="primary" size="small" plain @click="sell()">卖出股票</el-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import navMenu from "@/components/navmenu.vue";
import vueKline from "vue-kline";
import { userStockInfo } from "@/api/user"
import { buyStock, sellStock } from "@/api/trade"
import { stockHistory } from "@/api/stock";

export default {
  name: "stockPage",
  components: {
    'nav-menu': navMenu,
    vueKline,
  },
  data(){
    return{
      stockId: 0,
      stockIdStr: null,
      buyNum: 1,
      sellNum: 1,
      klineData: {},
      stockInfo: {
        "stockName": "百度",
        "stockCode": 58,
        "price": 41,
        "closingPrice": 94,
        "maxPrice": 43,
        "minPrice": 2,
        "changePercent": 12,
        "volume": 73
      },
      tradeInfo: {
        "userName": "ghw",
        "balance": 50000,
        "tradeInfo": [
          {
            "stockId": 1,
            "stockName": "百度",
            "cost": 3200,
            "quantity": 10,
            "price": 30,
            "value": 3000,
            "earn": 0,
            "profit": -200
          }
        ]
      },
      klineParams:{
        width: 920,
        height: 400,
        theme: "light",            // 主题颜色
        language: "zh-cn",        //语言
        ranges: ["1w", "1d", "1h", "30m", "15m", "5m", "1m", "line"],  // 聚合选项
        intervalTime: 50000,       // k线更新周期 毫秒
        depthWidth: 50,           // 深度图宽度
        count: 2                  //显示指标数量 默认两个
      }
    }
  },
  created() {
    this.stockIdStr = this.$route.params.id;
    this.stockId = parseInt(this.$route.params.id);
    this.getInfo();
    this.getStockInfo();
  },
  mounted(){
    this.refreshKlineData(900000);// 进入页面时执行,默认聚合时间900000毫秒(15分钟)
  },
  methods:{
    requestData(){
      stockHistory(this.stockIdStr).then(res => {
        this.klineData = res.data.klines;
        this.$refs.callMethods.kline.chartMgr.getChart().updateDataAndDisplay(res.data.klines.lines); //强制更改缓存中的lines值,防止显示不同步
      })
    },
    refreshKlineData(option){
      if (option===900000){ //如果时间等于15分钟
        this.requestData();
      }
    },
    getStockInfo(){
      stockHistory(this.stockIdStr).then(res => {
        this.stockInfo = res.data.stockSummary;
      })
    },
    getInfo(){
      userStockInfo(this.stockIdStr).then(res => {
        this.tradeInfo = res.data;
      })
    },
    buy(){
      buyStock(this.stockId, this.buyNum).then(() => {
        this.$message.success("Stock purchase successful");
      }).catch((err) => {
        console.log(err);
        this.$message({
          message: 'Not enough balance',
          type: 'warning'
        });
      })
    },
    sell(){
      sellStock(this.stockId, this.sellNum).then(() => {
        this.$message.success("Stock sale successful");
      }).catch((err) => {
        console.log(err);
        this.$message({
          message: 'Not enough shares',
          type: 'warning'
        });
      })
    },
  },
}
</script>

<style scoped>
.stock-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}
.el-divider{
  background-color: #89c4fd;
  height: 1.2px;
}
.el-input-number{
  margin-right: 8px;
}
.stock{
  width: 60%;
}
.user-info{
  width: 60%;
  margin-top: 5px;
  margin-bottom: 30px;
}
.info{
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin-top: 40px;
}
.trade{
  width: 60%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
</style>