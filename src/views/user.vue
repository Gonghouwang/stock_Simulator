<template>
    <div>
        <nav-menu></nav-menu>
        <div class="user-dashboard">
            <!-- 账户信息 -->
            <div class="account-overview">
                <h2>{{ account.username }}</h2>
                <div>
                    <p>总资产: {{ account.balance }} 元</p>
                    <p>总收益: {{ account.profit }} 元</p>
                </div>
            </div>

            <!-- 持仓信息 -->
            <div class="stock-holdings">
                <h2>持仓信息</h2>
                <el-table :data="holdings">
                    <el-table-column prop="stockName" label="股票名称"></el-table-column>
                    <el-table-column prop="quantity" label="当前持仓量"></el-table-column>
                    <el-table-column prop="price" label="当前价格"></el-table-column>
                    <el-table-column label="盈亏">
                        <template slot-scope="scope">
                            <!-- 根据盈亏动态设置样式 -->
                            <span :class="priceClass(scope.row.profit)">
                                {{ scope.row.profit }} 元
                            </span>
                        </template>
                    </el-table-column>
                </el-table>
            </div>

            <!-- 交易记录 -->
            <div class="transaction-history">
                <h2>交易记录</h2>
                <el-collapse>
                    <el-collapse-item v-for="(transaction, index) in transactions" :title="transaction.stockName"
                        :key="index">
                        <div>
                            <p>股票代码：{{ transaction.stockId }}</p>
                            <p>类型: {{ transaction.tradeType }}</p>
                            <p>价格: {{ transaction.tradePrice }}</p>
                            <p>数量: {{ transaction.tradeQuantity }}</p>
                            <p>交易时间: {{ transaction.tradeDate }}</p>
                        </div>
                    </el-collapse-item>
                </el-collapse>
            </div>
        </div>
    </div>
</template>

<script>
import { user, userInfo, userStockTradeHistory } from "@/api/user";
import navMenu from "@/components/navmenu.vue";
export default {
    name: "userPage",
    components: {
        'nav-menu': navMenu,
    },
    data() {
        return {
            account: {
                username: '用户名',
                balance: 100000, // 总资产
                profit: 12000    // 总收益
            },
            holdings: [
                {
                    "stockId": 1,
                    "stockName": "美团",
                    "cost": 8000,
                    "quantity": 40,
                    "price": 25,
                    "value": 10000,
                    "earn": 1000,
                    "profit": 3000
                },
                {
                    "stockId": 2,
                    "stockName": "腾讯",
                    "cost": 9000,
                    "quantity": 20,
                    "price": 40,
                    "value": 8000,
                    "earn": 0,
                    "profit": -1000
                }
            ],
            transactions: [
                {
                    "userId": 1,
                    "stockId": 2,
                    "stockName": "美团",
                    "tradeType": 0,
                    "tradeQuantity": 29,
                    "tradePrice": 29,
                    "tradeDate": "2023-11-22"
                },
                {
                    "userId": 1,
                    "stockId": 3,
                    "stockName": "腾讯",
                    "tradeType": 1,
                    "tradeQuantity": 39,
                    "tradePrice": 58,
                    "tradeDate": "2025-05-19"
                }
            ],
        };
    },
    created() {
        this.getUserInfo();
        this.getUser();
        this.getHistory();
    },
    methods: {
        priceClass(profit) { //涨跌颜色
            if (profit < 0) {
                return 'price-down';
            } else {
                return 'price-up';
            }
        },
        getUserInfo() { //获取用戶简要信息
            userInfo().then(response => {
                this.holdings = response.tradeInfo;
            })
        },
        getUser() { //获取用戶简要信息
            user().then(response => {
                this.account = response.data;
            })
        },
        getHistory() { //交易历史
            userTradeHistory().then(response => {
                this.transactions = response.data.history;
            })
        }
    }
}
</script>

<style scoped>
.user-dashboard {
    display: flex;
    flex-wrap: wrap;
    padding: 20px;
    justify-content: center;
}

.account-overview {
    background-color: #f0f0f0;
    padding: 16px;
    border-radius: 8px;
    margin-bottom: 20px;
    min-width: 80%;
    max-width: 80%;
}

.stock-holdings {
    margin-top: 20px;
    min-width: 80%;
    max-width: 80%;
}

.transaction-history {
    margin-top: 20px;
    min-width: 80%;
    max-width: 80%;
}

.price-down {
    color: #01b301;
    /* 当 changePercent 是负数时，价格显示绿色 */
}

.price-up {
    color: red;
    /* 当 changePercent 是正数时，价格显示红色 */
}
</style>
