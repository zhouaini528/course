### 小白视频教程文档

#### 前言

1：让完全不懂程序开发的小白如何几小时内学会量化交易？

2：没有复杂的逻辑、复杂代码命令、复杂的环境。

3：选择什么样的程序开发语言？



### 一：对什么平台量化，需要什么样的环境？

1：数字货币

2：一台服务器以及PHP应用软件

```shell script
#先更新系统
apt-get update


#安装系统php
apt-get install php7.4 php7.4-curl

#查看是否安装正确
php -v
```

3：程序输出一个hello world

```shell script
#nano 是linux下的自带命令，可修改可创建
nano hello.php
```

输入以下代码
```php script
<?php
echo “hellow world”;
```


### 二：如何获取交易所盘口数据和下单命令

1：选择数字货币哪家交易所？

Huobi okex  binacne

2：如何获取盘口行情数据？

```php
//php自带的函数
file_get_contents();
```

3：如何下单？

火币交易所创建API

火币官方 Sdk [https://github.com/huobiapi/REST-PHP-demo](https://github.com/huobiapi/REST-PHP-demo)





### 三：如何完成一个简单定投策略

1：无任何限制简单定投，比如：每6个小时定投

```php
//php自带函数 默认秒单位
sleep(60*60*6);
```

2：某一时刻定投，比如每天12:30点准时定投，相隔24小时


3：如何知道自己的程序有没有执行？下单成功后是否记录？

```php
//php自带时间函数
$time=time();

//php自带格式化函数
echo date(‘Y-m-d H:i:s’,$time);
```






### 四：如何根据盘口行情进行下单策略

1：简单的根据盘口数据开仓

2：开仓后如何盈利10%后平仓。   


### 五：如何让程序一直后台运行？

### 六：预先善其事必先利其器

