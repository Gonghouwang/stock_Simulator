<template>
    <div>
        <nav-menu></nav-menu>
        <div class="user-dashboard">
            <!-- 账户信息 -->
            <div class="account-overview">
                <h2>账户总览</h2>
                <div>
                    <p>总资产: {{ account.totalAssets }} 元</p>
                    <p>总收益: {{ account.totalProfit }} 元</p>
                </div>
            </div>

            <!-- 持仓信息 -->
            <div class="stock-holdings">
                <h2>持仓信息</h2>
                <el-table :data="holdings">
                    <el-table-column prop="name" label="股票名称"></el-table-column>
                    <el-table-column prop="quantity" label="当前持仓量"></el-table-column>
                    <el-table-column prop="currentPrice" label="当前价格"></el-table-column>
                    <el-table-column prop="profitLoss" label="盈亏"></el-table-column>
                </el-table>
            </div>

            <!-- 交易记录 -->
            <div class="transaction-history">
                <h2>交易记录</h2>
                <el-collapse>
                    <el-collapse-item v-for="(transaction, index) in transactions" :title="transaction.name"
                        :key="index">
                        <div>
                            <p>交易时间: {{ transaction.date }}</p>
                            <p>类型: {{ transaction.type }}</p>
                            <p>价格: {{ transaction.price }}</p>
                            <p>数量: {{ transaction.quantity }}</p>
                        </div>
                    </el-collapse-item>
                </el-collapse>
            </div>
        </div>
    </div>
</template>

<script>
import navMenu from "@/components/navmenu.vue";
export default {
    name: "userPage",
    components: {
        'nav-menu': navMenu,
    },
    data() {
        return {
            account: {
                totalAssets: 100000, // 总资产
                totalProfit: 12000    // 总收益
            },
            holdings: [
                { name: '股票A', quantity: 100, currentPrice: 25, profitLoss: 500 },
                { name: '股票B', quantity: 200, currentPrice: 50, profitLoss: -1000 }
            ],
            transactions: [
                { name: '股票A', date: '2023-09-01', type: '买入', price: 20, quantity: 100 },
                { name: '股票B', date: '2023-09-15', type: '卖出', price: 50, quantity: 50 }
            ],
        };
    },
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
</style>
