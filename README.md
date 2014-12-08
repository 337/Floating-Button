#ELEX移动游戏发行解决方案

##帐号解决方案
ELEX把帐号逻辑开放给CP去选择，并不刻意要求CP对接任何帐号系统，发行运营同学同CP游戏策划人员可以根据游戏情况，自行选择如下的帐号解决方案或者多个方案组合的方式进行实施

* CP自己的帐号系统
* 直接玩（设备ID注册）
* Facebook帐号登录
* Game Center帐号解决方案
* Google Play帐号解决方案

##游戏内悬浮按钮
游戏内悬浮按钮提供服务器端控制的悬浮按钮，运营人员可以定义悬浮按钮展开后所展示的内容，常规的应用场景有

* 支持第三方支付方式的网页版支付
* Facebook 粉丝页的推广
* 任何基于Web页面形式的内容的推广

如果需要使用上述功能，需要集成ELEX悬浮按钮组件库，各个平台的整合说明如下：

###iOS

####下载iOS组件文件

下载链接：[https://github.com/337/Floating-Button/releases/download/ios1.0/Floatint-Button_1.0_iosSDK.zip](https://github.com/337/Floating-Button/releases/download/ios1.0/Floatint-Button_1.0_iosSDK.zip)

####在项目中引入相关组件

2. 引入 `SuspensionButton.bundle`，`SuspensionButton.h` 和  `libSuspensionButton.a`

2. 引入 `CoreTelephony.framework`

####初始化

在如下函数中追加`SuspensionButton_init("", uid);`来完成初始化， uid为用户在游戏中的唯一标识，如果用户支付，此uid将会是支付完成通知中的用户id。追加 `SuspensionButton_size(50);`来指定悬浮按钮的大小。

	- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions{}

完成之后应当类似于：

	- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    	SuspensionButton_init("", "123456");  //此处假定用户UID为 123456
    	SuspensionButton_size(50);
    	return YES;
	}
	
####显示悬浮按钮
完成初始化之后，游戏开发者可以在自己觉得适宜的时机，调用`SuspensionButton_show();` 方法来将悬浮按钮展现出来。

*因为悬浮按钮是否可以展示以及在何种逻辑下展示是由服务器端控制的，所以调用此方法之后有可能不会发生任何效果，如果发生此种情况，请联系ELEX方检查服务器端的配置是否正常*

####如果游戏被切换到后台
游戏需要通知悬浮按钮自己将进入后台，具体的方法是在 `- (void)applicationDidEnterBackground:(UIApplication *)application` 函数中调用`SuspensionButton_didAppEnterBackground();`

完成后应当类似于：

	- (void)applicationDidEnterBackground:(UIApplication *)application {
    	// 通知悬浮按钮，游戏已经暂停
    	SuspensionButton_didAppEnterBackground();
	}

####如果游戏被切换到前台
游戏需要通知悬浮按钮自己已经从后台恢复倒前台，具体的方法是在 `- (void)applicationDidBecomeActive:(UIApplication *)application` 函数中调用`SuspensionButton_didAppBecomeActive();`

完成后应当类似于：

	- (void)applicationWillResignActive:(UIApplication *)application {
    	// 通知悬浮按钮，游戏已经运行
    	SuspensionButton_didAppBecomeActive();
	}

我们出具了一个可以完整体现上述接入结果的Demo，开发者可以下载参考。 下载链接：[https://github.com/337/Floating-Button/releases/download/ios1.0/Floatint-Button_1.0_iosSDK.zip](https://github.com/337/Floating-Button/releases/download/ios1.0/Floatint-Button_1.0_iosSDK.zip)。

### Android

####下载组件包

下载链接：[https://github.com/337/Floating-Button/releases/download/android1.0/Floatint-Button_1.0_androidSDK.zip](https://github.com/337/Floating-Button/releases/download/android1.0/Floatint-Button_1.0_androidSDK.zip)

####在项目中引用组件

在游戏项目中引入`SuspensionButton`

####初始化  
	
调用`SuspensionButton.init(this, "", "$UID", $SIZE);` 函数初始化悬浮按钮。第二个参数留空即可,第三个参数是用户的唯一标识，第三个参数是悬浮窗尺寸。

####在游戏应用的4个生命周期中调用相应函数	
#####onResume
		
			SuspensionButton.getInstance().onResume();
			
#####onPause：
		
			SuspensionButton.getInstance().onPause();
			
#####onDestroy：
		
			SuspensionButton.getInstance().destroy();
			
#####onRestart：
		
			SuspensionButton.getInstance().onRestart(this);
			
###其他接口
如果悬浮按钮中需要支持网页版支付，CP还需要实现支付完成通知接口，该接口与OFF-APP支付中需要的接口相同，请参照OFF-APP支付部分。

###Google Play IAP接入Demo
游戏新接入Google Play IAP时，可以参考官方Demo。

下载地址：[https://github.com/337/Floating-Button/releases/download/gpdemo/GooglePlayDemo.zip](https://github.com/337/Floating-Button/releases/download/gpdemo/GooglePlayDemo.zip)

该工程中对关键代码添加了注释，可供参考。

使用Googleplay内购功能需要设备中安装了完整的Google服务，中国的机器一般不会安装Google服务，可以通过刷机软件刷入Google服务。

中国的Play商店也不能使用内购功能。需要清空Google服务和Play商店的数据，然后挂上国外vpn，再进入Play商店，如果能看到付费应用了，这时这台设备才可以使用内购功能。

更详细的说明可以参考官方文档：[https://developer.android.com/google/play/billing/api.html](https://developer.android.com/google/play/billing/api.html)

接入完成后的支付测试可以参考：[https://developer.android.com/google/play/billing/billing_testing.html](https://developer.android.com/google/play/billing/billing_testing.html)

##OFF-APP 支付解决方案
ELEX为游戏提供OFF-APP支付解决方案，使游戏可以在Apple IAP和Google Play IAP之外有其他的支付选择，此项功能默认关闭，如需开启，需要提供两个接口来实现相应功能。

此方案用户端流程如下：

用户用浏览器打开游戏的支付页面，输入自己在游戏中的唯一标识（根据游戏不同，可以是用户名，UID，角色名，自己城堡的ID，等等），337以用户输入的内容请求CP端『用户信息查询接口』，CP返回相应用户的信息，337根据信息引导用户完成支付。支付完成后，337会通过服务器端通知的方式告知游戏服务器用户的充值信息。

###用户信息查询接口
此接口输入为用户UID，输出为用户信息（玩过的服务器，相应服务器中的角色名称等）。接口规格如下；

####请求方式
GET
####请求参数
* user_id: 用户可感知的唯一标识，比如CP端用户帐号的用户名，用户UID等。

####返回值

Json格式，内容如下：


值 | 说明 | 内容范围
------------ | ------------- | ------------
status | 返回值状态  | -1：用户不存在，1：查询成功
uid | 游戏希望在支付通知中收到的UID  | 
list | 用户已经激活的服务器列表  | name:服务器名；status：服务器状态（1正常，-1维护）；server_id：服务器ID；role_info：用户在该服务器的信息（描述见下表）

role_info 字段说明

值 | 说明 | 内容范围
------------ | ------------- | ------------
info | 用户角色信息  | nickname：用户昵称；level：用户等级；role_id：用户角色id
status | 用户状态  |1：正常用户；-1：非正常用户

实例如下：

	
	
	{
		status: 1,
		uid: "123456",
		list: [
			{
				name: "繁體測試服",
				status: 1,
				server_id: 998,
				role_info: [
					{
						status: 1,
						info: {
							nickname: "小明",
							level: 10,
							role_id: 998000004
						}
					}
				],
			}
		]
	}



###支付完成通知接口（Server 2 Server）
当用户完成付款之后，337平台需要将付款结果发送给CP，CP需要给用户账户增加相应的虚拟币或道具，并返回处理结果。

337后端支付的通知采用两次验证的方式，整个流程如下：

1. 337发起支付完成通知，带着一系列参数请求CP提供的支付回调接口
2. CP端收到调用请求，取下参数，将参数回传给337支付平台的验证服务
3. 337支付平台验证参数，返回验证结果
4. CP端收到验证结果，如果验证通过，则进行虚拟币和虚拟道具颁发工作。颁发完毕返回`3,{用户的UID}`告知337支付平台处理完毕。
5. 流程结束。

####请求方式
POST

####请求参数

参数				| 说明 
------------ 	| ------------- 
trans_id 		| 唯一标示这次支付的订单号,重复单号不用处理
amount		 	| 用户购买游戏币数量，开发者需要给用户的游戏账户加上相应的数量。
user_id 		| 购买发起的游戏用户标识，同登录时的sig_user
role_id			| 用户游戏角色的ID，适用于单服多角色的场景
timestamp 		| 请求时间戳，避免重复调用攻击
gross			| 扣除渠道费用之前的交易金额
currency		| 货币单位，例如:USD,EUR,BRL
channel			| 支付渠道名称
pay_type		| 支付类型，web/mobile, 用以区分支付来源是手机应用内充值还是其他充值方式
vip				| 布尔值，用户是否享有了平台VIP折扣
custom_data		| 供CP和337自由定义的自定义参数，按照提交原样返回
version			| 9

> * `gross`只是作为参考使用，不作为对账依据。
> * 在某些支付渠道，337平台无法获得用户实际支付金额，所以`gross`参数可能为0。
> * 对于短信等费率超高的渠道，337平台会进行溢价，请不要以`gross`的值进行虚拟币计算，虚拟币数量请以`amount`参数为准。

####参数验证
验证服务地址：https://pay.337.com/payelex/api/callback/verify.php

CA文件（PHP可能需要）：http://doc.xingcloud.com/download/attachments/4195503/verisign_ca.crt?version=1&modificationDate=1327048502000

接受请求方式：POST/GET

验证返回结果：

* OK： 交易信息正确，请进行后续处理。
* 其他返回，交易信息错误，请放弃处理。


####返回值

CP应当返回		| 说明 
------------ 	| ------------- 
3,null 			| 处理失败
3,94a0acb127ef8ee8c925e3944941ce5e		 	| 处理失败，用户不存在
3,$user_id  	| 处理成功

####代码示例(PHP)
	
	<?php
	$trans_id = $_REQUEST ["trans_id"];
	$user_id = $_REQUEST ["user_id"];
	$amount = $_REQUEST['amount'];
	$gross = $_REQUEST['gross'];
	$currency = $_REQUEST['currency'];
	$channel = $_REQUEST['channel'];
 
	ob_clean();
	//To check if the transaction exists in db.
	//Yes means the transactions has been successfully processed. Just return OK status
	$exist = is_trans_exist($trans_id);
	if($exist) {
		echo '3,'.$user_id;
		return;
	}
 
	//to verify the transaction towards payelex server.
	$res = check_payelex_transaction($trans_id, $user_id, $amount, $gross, $currency, $channel);
	if(!$res) {
		echo "3,null";
		return;
	}
 
	//retrieve the user from db.
	$user = find_user_from_db();
	if ($user == null) {
		echo '3,94a0acb127ef8ee8c925e3944941ce5e';
		return;
	}
 
	//recharge the user with the deserved game coins.
	if(add_coins($_REQUEST)) {
		echo '3,'.$user_id;
		return;
	}
	
	echo "3,null";
	
	function check_payelex_transaction($trans_id, $user_id, $amount, $gross, $currency, $channel) {
		$ch = curl_init();
		curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, true);
		curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, 1);
		//verisign_ca.crt is the public certificate from
		//VeriSign(It is the biggest Certificate Authority which issue XingCloud client certificate)
		//verisign_ca.crt must be located at the same directory as this PHP code are.
		curl_setopt($ch, CURLOPT_CAINFO, 'verisign_ca.crt');
		curl_setopt($ch, CURLOPT_HTTPHEADER, array("Content-Type: application/x-www-form-urlencoded"));
		curl_setopt($ch, CURLOPT_URL, 'https://pay.337.com/payelex/api/callback/verify.php');
		curl_setopt($ch, CURLOPT_POST, true);
		curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
		
		$params = array(
                'trans_id'=>$trans_id,
                'user_id'=>$user_id,
                'amount'=>$amount,
                'gross'=>$gross,
                'currency'=>$currency,
                'channel'=>$channel
        );
        
        curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($params));
        
        $result = curl_exec($ch);
        curl_close($ch);
        $result = trim($result);
        if ($result === 'OK') return true;
        return false;
	}

###（可选）提交订单接口
个别CP可能需要用户在付款之前先在自己的系统中为用户创建订单，337支持这样的流程，CP需要额外提供一个提交订单接口，通过这个接口，当用户准备用OFF-APP付款时，337会提交用户的订单信息。

####请求方式
GET/POST

####请求参数
值 | 说明 | 内容范围
------------ | ------------- | ------------
orderid | 订单ID  | 
uid | 用户ID  |
amount | 充值游戏币数量  |
server_id | 服务器id  |

####返回值
值 | 说明 | 内容范围
------------ | ------------- | ------------
status | 返回值状态  | -1：订单创建失败；1：订单创建成功