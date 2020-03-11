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
        "wallet_password":"wallet_password"
    }
```


#### 配置代理
1.在 site-packages/pyjsonrpc/http.py的第25行增加如下
注意替换ip 和端口

2.然后在config文件中配置
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
