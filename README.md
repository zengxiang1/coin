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





#### eth 
首先 在启动参数加上 --allow-insecure-unlock
然后配置密码 
```
def eth():
    return {
        #rpc链接
        "url": "http://ip:port",
                #rpc用户名
        "user": "123",
                #rpc密码
        "password": "123",
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
1.查询余额
```
python main.py -c eth -p list
```
2. 归总（切记不要弄错归总地址， 请再三检查）
```
python main -c eth -p send
```

#### erc20

配置
出于方便erc20的地址默认归总到eth配置到总地址上面

```
def erc20():
    return {
        #   代币地址 默认为usdt地址
        "address": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
        #   代币的decimal 从etherscan可以获取。usdt是6
        "decimal": 6,
        #   最小代币金额
        "min": 0.0001,
        #   手续费地址 手续费地址
        "feeAddress": "0x3490e84c3783ad13e86f25c3ec64af8e1c68b74a"
    }
```
流程 
1.列出erc20代币地址以及数量
```
python main.py -c erc20 -p list
```
可以查看每个地址代币情况以及总共多少代币
2.给其中一个地址打手续费，然后记住这个地址 , 将这个地址填入 上述配置中的 feeAddress (可以多打点， 大概0.5个左右 打完手续费之后用eth归总命令执行归总) 然后执行
```
python main.py -c erc20 -p fee
```

3.去https://etherscan.io/address/(feeAddress)  feeAddress替换上述中的手续费地址， 查看是否所有手续费已经到账(没有pending的交易)  如果都已经到账， 请执行如下命令
```
python main.py -c erc20 -p send
```
请等待代币到账.....
到账后请继续执行eth归总命令回收手续费




