# 如何使用Jupyter Notebook进行现货交易

## 准备工作与环境搭建

使用Jupyter Notebook进行现货交易需要完成以下基础设置：

👉 [立即获取加密交易API接口文档](https://bit.ly/okx_welcome)

### 安装Python开发环境
1. 安装Anaconda或Miniconda管理工具
2. 创建独立虚拟环境：`conda create -n okx-trade python=3.9`
3. 激活环境：`conda activate okx-trade`

### 安装核心依赖包
```bash
pip install python-okx jupyter pandas numpy
```

### 启动Jupyter Notebook
```bash
jupyter notebook
```

## API密钥配置指南

### 创建API密钥步骤
1. 登录OKX账户
2. 进入「API管理」页面
3. 创建新密钥并配置：
   - 权限设置：勾选「交易」权限
   - 绑定IP（可选）：增强安全性
4. 保存密钥信息到安全位置

```python
# API配置示例
api_key = "your_api_key"
secret_key = "your_secret_key"
passphrase = "your_passphrase"
```

## 核心交易模块解析

### 初始化交易接口
```python
import okx.Trade as Trade
tradeAPI = Trade.TradeAPI(api_key, secret_key, passphrase, False, "1")
```

### 市场数据获取
```python
import okx.MarketData as MarketData
marketData = MarketData.MarketAPI(flag="1")
tickers = marketData.get_tickers(instType="SPOT")
```

## 交易策略实施

### 现货交易模式选择
| 模式类型             | 参数设置       | 适用场景               |
|----------------------|----------------|------------------------|
| 现货模式             | tdMode='cash'  | 单币种现货交易         |
| 跨币种保证金模式     | tdMode='cross' | 多币种组合保证金交易   |

### 订单类型对比
```python
# 限价单示例
tradeAPI.place_order(
    instId="BTC-USDT",
    tdMode="cash",
    side="buy",
    ordType="limit",
    px="30000",
    sz="0.001"
)
```

```python
# 市价单示例
tradeAPI.place_order(
    instId="BTC-USDT",
    tdMode="cash",
    side="buy",
    ordType="market",
    sz="100"
)
```

## 风险控制体系

### 账户余额监控
```python
import okx.Account as Account
accountAPI = Account.AccountAPI(api_key, secret_key, passphrase, False, "1")
balance = accountAPI.get_account_balance()
```

### 订单管理功能
```python
# 查询订单状态
tradeAPI.get_order(instId="BTC-USDT", ordId="123456")

# 取消未成交订单
tradeAPI.cancel_order(instId="BTC-USDT", ordId="123456")
```

## 高级功能应用

### 批量订单处理
```python
orders = [
    {
        "instId": "BTC-USDT",
        "tdMode": "cash",
        "side": "buy",
        "ordType": "limit",
        "px": "30000",
        "sz": "0.001"
    },
    {
        "instId": "ETH-USDT",
        "tdMode": "cash",
        "side": "buy",
        "ordType": "limit",
        "px": "2000",
        "sz": "0.01"
    }
]
tradeAPI.place_multiple_orders(orders)
```

### 历史订单查询
```python
# 查询最近7天订单
tradeAPI.get_orders_history(instType="SPOT")

# 查询历史归档订单
tradeAPI.get_orders_history_archive(instType="SPOT")
```

## 常见问题解答（FAQ）

**Q：如何处理API调用频率限制？**
A：建议采用以下策略：
1. 使用`time.sleep()`控制请求间隔
2. 合并批量请求
3. 监控`X-OKX-RATELIMIT`响应头
4. 申请提高API限频额度

**Q：测试环境与生产环境如何切换？**
A：通过修改flag参数实现：
- 测试环境：`flag="1"`
- 生产环境：`flag="0"`
修改时请确保API密钥对应环境权限

**Q：如何实现自动化交易策略？**
A：建议架构：
1. 使用`schedule`模块定时执行策略
2. 集成`TA-Lib`进行技术分析
3. 使用`backtrader`进行策略回测
4. 部署到云服务器实现7x24运行

👉 [获取专业量化交易解决方案](https://bit.ly/okx_welcome)

## 交易模式深度解析

### 统一账户模式对比
| 模式等级 | 参数值 | 特点说明                     |
|----------|--------|------------------------------|
| 简易模式 | "1"    | 单一账户结构，适合新手       |
| 单币保证金 | "2"  | 独立保证金管理，灵活性强     |
| 多币保证金 | "3"  | 组合保证金体系，风险分散     |
| 组合保证金 | "4"  | 全市场风险对冲，专业级方案   |

### 高级订单参数
```python
# 带附加参数的订单示例
tradeAPI.place_order(
    instId="BTC-USDT",
    tdMode="cash",
    side="buy",
    ordType="limit",
    px="30000",
    sz="0.001",
    tgtCcy="base_ccy",  # 指定交易币种
    clOrdId="ORDER_001" # 自定义订单ID
)
```

## 数据分析与可视化

### 市场数据处理
```python
import pandas as pd
df = pd.DataFrame(tickers['data'])
df[['instId', 'last', 'vol24h']].sort_values('vol24h', ascending=False).head(10)
```

### 交易记录分析
```python
history = tradeAPI.get_orders_history(instType="SPOT")
df = pd.DataFrame(history['data'])
df[['instId', 'side', 'px', 'sz', 'pnl']].astype({'sz': 'float', 'px': 'float'})
```

👉 [体验专业级交易数据分析工具](https://bit.ly/okx_welcome)