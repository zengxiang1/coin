### 脚本使用指南

#### 1. 配置
1. 先配置 config.py 文件
2. 按照要求配置
```
# coding: utf-8
def btc():
    return {
        # btc节点url
        "url": "http://127.0.0.1:8332:",
        # btc节点用户名
        "user": "admin",
        #btc节点密码
        "password": "admin",
        #btc归总地址
        "total": "1BtjjiHe7bFor3z1fp1rYSr993eUZtSSbu",
        #btc节点域名
        "host" : "127.0.0.1",
        #btc归总最少金额()
        "minbtc": 0,
        #手续费地址
        "fee_sender": "1BtjjiHe7bFor3z1fp1rYSr993eUZtSSbu",
        #钱包密码
        "wallet_password":"wallet_password",
        #最少usdt归总
        "minusdt": 10,
        # 自定义手续费设置 默认50     如果从网上自动获取直接删掉 ， 如果自定义不知道填多少可以从imtoken上获取（转账的下面有乌龟和兔子中间那个数）
        "fee_rate": 50
    }
```


#### 配置代理

1.然后在config文件中配置
注意替换ip 和端口
```
def common():
    return {
        "proxy": {
            "host": "127.0.0.1",
            "port": 1081
        }
    }
```
如果不想要代理
就保留如下
```
def common():
    return {
        
    }
```
#### 3. 安装依赖
```
pip install -r requirement.txt
```
#### usdt
1.获取地址余额列表
```
python main.py -c usdt -p list
```
2. 归总
流程
先在此钱包中创建一个新的地址
```
omnicore-cli -datadir =<你的omnicore datadir> getnewaddress
```
然后转入足量的手续费（使用完毕之后可以转回来）
```
python main.py -c usdt -p send
```
如果报500 错误  则是手续费不足




btc
1.获取地址余额列表
```
python main.py -c btc -p list
```
2归总
```
python main.py -c btc -p send
```

归总配置
wallet/btc.py 第98行 默认
```

tx_hex = self.client.createrawtransaction(inputs, outputs)
# version < 0.7.0
signed_hex = self.client.signrawtransaction(tx_hex)['hex']
# version >= 0.7.0
# signed_hex = self.client.signrawtransactionwithwallet(tx_hex)['hex']
```

如果这样提示 method not found 就改成如下
```
tx_hex = self.client.createrawtransaction(inputs, outputs)
# version < 0.7.0
#signed_hex = self.client.signrawtransaction(tx_hex)['hex']
# version >= 0.7.0
signed_hex = self.client.signrawtransactionwithwallet(tx_hex)['hex']

```

1.获取地址余额列表接口
1.获取地址余额列表



eth 
首先 在启动参数加上 --allow-insecure-unlock
然后配置密码 
```
def eth():
    return {
        #rpc链接
        "url": "http://13.229.231.180:8546",
                #rpc用户名
        "user": "sey",
                #rpc密码
        "password": "123456",
                #rpc钱包密码
        "wallet_password": "123456",
                #最小额度
        "min": "0.0001",
                #归总地址
        "total": "0xD3D66b1f124a2C36647762A4b89F280F19dFe3D3",
                #gas price https://etherscan.io/gastracker 默认15是安全gas范围
        "gasPrice": 15
    }
```
