### 小白视频教程文档


#### 前言

1：让完全不懂程序开发的小白如何几小时内学会量化交易？

2：没有复杂的逻辑、复杂代码命令、复杂的环境。

3：选择什么样的程序开发语言？



### 一：对什么平台量化，需要什么样的环境？

1：数字货币

2：一台服务器以及PHP应用软件

购买服务器前建议你去领优惠券，也许还可以给你优惠点[点我领取](https://promotion.aliyun.com/ntms/yunparter/invite.html?userCode=xmsff8ku)

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
echo "hellow world";
```

[视频地址:https://www.bilibili.com/video/BV1rk4y117TR?p=1](https://www.bilibili.com/video/BV1rk4y117TR?p=1)

### 二：如何获取交易所盘口数据和下单命令

1：选择数字货币哪家交易所？

[Huobi](https://github.com/zhouaini528/huobi-php)

[Okex](https://github.com/zhouaini528/okex-php)

[Binance](https://github.com/zhouaini528/binance-php)

2：如何获取盘口行情数据？

```php
//php自带的远程获取数据函数
file_get_contents();

//完整代码  每2秒获取一次
while(1){
    print_r(file_get_contents('https://api.huobi.pro/market/depth?symbol=btcusdt&type=step2&depth=5'));
    sleep(2);
}
```

3：如何下单？

火币交易所创建API

火币官方 Sdk [https://github.com/huobiapi/REST-PHP-demo](https://github.com/huobiapi/REST-PHP-demo)

创建demo.php 和 lib.php

```php
//定义参数
define('ACCOUNT_ID', '11046977'); // your account ID 
define('ACCESS_KEY','347eb106-87db8074-83adcc7b-xxxxxxx'); // your ACCESS_KEY
define('SECRET_KEY', '44559ed2-xxxxxxx-42dbede5-xxxxxx'); // your SECRET_KEY

include "lib.php";

//实例化类库
$req = new req();

//下单方式
print_r($req->place_order(11046977,5,7,'eosusdt','sell-limit'));
```

[视频地址:https://www.bilibili.com/video/BV1rk4y117TR?p=2](https://www.bilibili.com/video/BV1rk4y117TR?p=2)

### 三：如何完成一个简单定投策略

1：无任何限制简单定投，比如：每6个小时定投

```php
//php自带函数 默认秒单位
sleep(60*60*6);

//完整代码
define('ACCOUNT_ID', '11046977'); // your account ID 
define('ACCESS_KEY','347eb106-87db8074-83adcc7b-xxxxxxx'); // your ACCESS_KEY
define('SECRET_KEY', '44559ed2-xxxxxxx-42dbede5-xxxxxx'); // your SECRET_KEY
include "lib.php";
$req = new req();
while(1){
    print_r($req->place_order(11046977,5,7,'eosusdt','sell-limit'));
    //每6个小时定投
    sleep(60*60*6);
    //sleep(10);
}
```

2：某一时刻定投，比如每天12:30点准时定投，相隔24小时

```php
//php自带时间函数
$time=time();

//php自带格式化函数
echo date('Y-m-d H:i:s',$time);
```

3：如何知道自己的程序有没有执行？下单成功后是否记录？

```php
//完整代码
define('ACCOUNT_ID', '11046977'); // your account ID 
define('ACCESS_KEY','347eb106-87db8074-83adcc7b-xxxxxxx'); // your ACCESS_KEY
define('SECRET_KEY', '44559ed2-xxxxxxx-42dbede5-xxxxxx'); // your SECRET_KEY
include "lib.php";
$req = new req();
while(1){
    $time=time()+8*3600;
    $start=1596446400;

    if($time>$start){
        $t=$time-$start;
        //如果要24小时执行
        //if(is_int($t/86400)){

        //方便测试每10秒执行一次
        if(is_int($t/10)){
            print_r($req->place_order(11046977,5,7,'eosusdt','sell-limit'));
            echo '开仓：'.date('Y-m-d H:i:s',$time).PHP_EOL;
        }
    }

    //echo $time.PHP_EOL;
    echo date('Y-m-d H:i:s',$time).PHP_EOL;
    sleep(1);
}
```

[视频地址:https://www.bilibili.com/video/BV1rk4y117TR?p=3](https://www.bilibili.com/video/BV1rk4y117TR?p=3)

### 四：如何根据盘口行情进行下单策略

1：简单的根据盘口数据开仓

```php
//PHP自带json解析函数
json_decode();

//完整代码
define('ACCOUNT_ID', '11046977'); // your account ID 
define('ACCESS_KEY','347eb106-87db8074-83adcc7b-xxxxxxx'); // your ACCESS_KEY
define('SECRET_KEY', '44559ed2-xxxxxxx-42dbede5-xxxxxx'); // your SECRET_KEY
include "lib.php";
$req = new req();

while(1){
    $depth=file_get_contents('https://api.huobi.pro/market/depth?symbol=eosusdt&type=step2&depth=5');
    $depth=json_decode($depth,true);
    
    $buy=$depth['tick']['bids'][0][0];
    $sell=$depth['tick']['asks'][0][0];

    echo 'buy:'.$buy.PHP_EOL;
    echo 'sell:'.$sell.PHP_EOL;

    sleep(2);
}
```

2：开仓后如何盈利10%后平仓。

```php
//完整代码
define('ACCOUNT_ID', '11046977'); // your account ID 
define('ACCESS_KEY','347eb106-87db8074-83adcc7b-xxxxxxx'); // your ACCESS_KEY
define('SECRET_KEY', '44559ed2-xxxxxxx-42dbede5-xxxxxx'); // your SECRET_KEY
include "lib.php";
$req = new req();

//标记是否开仓
$tag=0;
//购买价格
$my_buy=3;
//购买数据
$my_count=10;
//卖出价格
$my_sell=$my_buy*1.1;

while(1){
    $depth=file_get_contents('https://api.huobi.pro/market/depth?symbol=eosusdt&type=step2&depth=5');
    $depth=json_decode($depth,true);

    $buy=$depth['tick']['bids'][0][0];
    $sell=$depth['tick']['asks'][0][0];

    echo 'buy:'.$buy.PHP_EOL;
    echo 'sell:'.$sell.PHP_EOL;

    //sell buy 为了测试的变量可删除
    $sell=2;
    $buy=4;

    if($buy>=$my_sell && $tag ==1){
        print_r($req->place_order(11046977,$my_count,$my_sell,'eosusdt','sell-limit'));
        echo '平仓成功'.PHP_EOL;
        break;
    }

    if($sell <= $my_buy &&  $tag==0){
        print_r($req->place_order(11046977,$my_count,$my_buy,'eosusdt','buy-limit'));
        $tag=1;
        echo '开仓成功'.PHP_EOL;
    }

    sleep(2);
}
```
[视频地址:https://www.bilibili.com/video/BV1rk4y117TR?p=4](https://www.bilibili.com/video/BV1rk4y117TR?p=4)


### 五：预先善其事必先利其器PHPstrom

1：下载安装windows版本phpstrom

2：SSH远程命令连接工具、SFPT文件同步

3：一些小技巧，[想要更深入了解的，看看这里](https://www.bilibili.com/video/BV1G4411g7bG)

[视频地址:https://www.bilibili.com/video/BV1rk4y117TR?p=5](https://www.bilibili.com/video/BV1rk4y117TR?p=5)

### 六：如何让程序一直后台运行？

1：如何知道程序一直在执行？

2：关闭了远程连接程序是否还会继续执行？

3：安装supervisor

```shell script
#linux 自带的命令，查看文件变化
tail -f

#安装
apt-get install supervisor
#启动设定的守护进程
supervisorctl start name:*
#停止所有守护进程
supervisorctl stop all
#开启所有守护进程
supervisorctl start all
#所有守护进程状态
supervisorctl status
#添加了进程守护配置文件才需要启动。视频中讲解的改了代码要启动是可以不用的
supervisorctl reload

#进程守护配置用例
[program:name]
process_name=%(program_name)s_%(process_num)02d
command=php /root/depth.php
stdout_logfile=/root/depth.log
```

### 七：强大的[exchanges-php](https://github.com/zhouaini528/exchanges-php)全球排前的交易所集合SDK。

1：如何安装exchanges-php？ composer 是什么？

2：如何使用exchanges-php？








