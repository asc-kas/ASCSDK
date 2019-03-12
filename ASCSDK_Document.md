# ASCSDK通用文档(ASCSDK Document) #

----------
### **简介(Introduction)** ###

> 欢迎使用ASCSSDK,ASCSSDK是渠道SDK以及广告SDK综合的打包工具,此工具接入简单，使用方便，无论是内购游戏，广告游戏，以及内购与广告混合模式的游戏，都可以很快的集成。
> 
> Welcome to use ASCSSDK. ASCSSDK is a comprehensive packaging tool for channel SDK and advertising SDK. This tool is easy to access and easy to use.

## **模块(Module)** ##
> * <font color=black size=4>**内购版(In purchasing version)**</font>
> * <font color=black size=4>**广告版(Advertising version)**</font>
> * <font color=black size=4>**数据统计(Data statistics)**</font>

## **内购模块(In purchase module)** ##
> 此模块适用于计费游戏，但是不包含广告。
>
> This module is suitable for billing games, but does not include advertising.

* ### <font color=red >必接功能(Necessary function)</font> ###
	* #### AndroidManifest.xml 配置 ####
		* AndroidManifest.xml文件里application下必须包含属性android:name="com.asc.sdk.ASCApplication"
			* The androidmanifest.xml file must contain attributes under application android:name="com.asc.sdk.ASCApplication"
		* 如果有第三方需要在Application里面初始化，可以继承ASCApplication，把自己继承的类填写到AndroidManifest.xml里面的android:name=“xxx.xxx.xxx.xxx.xxxApplication”属性。
			* If there is need to a third party in Application initialization, can inherit ASCApplication, his inheritance of class to fill out to AndroidManifest. The XML in the android:name=“xxx.xxx.xxx.xxx.xxxApplication” attribute.
		* 例如(For example)：<application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" 
        android:name="com.asc.sdk.ASCApplication">
	* #### ASCPlatform.java ####
		*  这个是所有游戏对接的接口类，所有的功能都是通过ASCPlatform调用对应的接口，实现交互功能。
		*  This is the interface class for all games. All functions are implemented by calling the corresponding interface of ASCPlatform.
	* #### 初始化SDK(Initialize SDK) ####
		* public void init(Activity context, final ASCInitListener callback)
		* ASCInitListener 回调监听(The callback listener)
			* public void onInitResult(int code, String msg)
				* 初始化SDK结果回调(Initialize the SDK result callback)
			* public void onLoginResult(int code, UToken data)
				* 登陆结果回调(Login result callback)
			* public void onSinglePayResult(int code, ASCOrder order)
				* 支付结果回调(Payment result callback)
			* public void onGiftResult(String msg);
				* 礼包兑换结果回调(Gift pack redemption results in a pullback)
		* onInitResult
			* 调用init()方法后，初始化之后会回调初始化回调方法
				* After initialization, the callback initializes the callback method
	* #### 登陆(Login) ####
		* public void login(Activity context)
		* 一般我会在初始化成功之后调用login()
			* I normally call login() after I've initialized it successfully
		* onLoginResult(int code, String msg)
			* 登陆之后会调用登陆回调接口(The login callback interface is called after logging in)
			  
	* #### 支付(Pay) ####
		* public void pay(Activity context, final PayParams data)
		* PayParams necessary
			* params->productId = "1";
			* params->productName = "1";
			* params->productDesc = "1";
			* params->price = 1;
			* params->buyNum = 1;
			* params->coinNum = 300;
			* params->serverId = "10";
			* params->serverName = "1111";
			* params->roleId = "asc_24532452";
			* params->roleName = "test";
			* params->roleLevel = 15;
			* params->vip = "v15";
			* params->payNotifyUrl = "1111";
			* params->extension = "1111";
		* onSinglePayResult(int code, ASCOrder order)
			* 支付完之后，返回游戏会调用这个支付结果回调
				* After the payment is made, the return game calls the payment result callback
			* code = 10 支付成功(Pay for success)
			* code = 11 支付失败(Pay for failure)
		* public void setPropDeliveredComplete(String orderID)
			* 道具下发成功通知，在支付成功后，玩家获取了对应的道具，然后通知服务器删除订单。如果不调用此方法，将会一直不会删除订单，会造成一直获取道具。
				* After the successful payment, the player gets the corresponding prop, and then notifies the server to delete the order. If this method is not called, the order will never be deleted and the item will always be fetched.
			* orderID 订单号
				* 在支付成功的时候onSinglePayResult(int code, ASCOrder order)，order里面获取订单号。
					* OnSinglePayResult (int code, ASCOrder order) on successful payment, where the order number is obtained.
				* 方法(method) order.getOrderID() 
	* #### 礼包兑换(Gift exchange) ####
		* public void exchangeGift(final String code)
		* 如果code是8位密码，不会调用礼包兑换的界面，直接服务器验证礼包码。这种情况适用于有自己的礼包兑换界面
			* If the code is 8-bit password, the interface of gift package exchange will not be called and the gift package code will be verified by the direct server. This applies to having your own gift exchange interface
		* 如果code不是8位密码，会调用礼包兑换界面，输入礼包兑换码，使用我们默认的礼包兑换界面。
			* If the code is not 8-bit password, the gift package exchange interface will be called. Enter the gift package exchange code and use our default gift package exchange interface.
		* onGiftResult(String msg)
			* 礼包兑换结果回调方法(Gift pack redemption results callback method)
			* msg是一个json的字符串，具体参数有(MSG is a json string with the following parameters)：
				* propNumber 道具数量
				* propType 道具类型
					* Golds 金币
					* Diamonds 钻石
					* Unlock 解锁
				* msg 消息
	* #### 评价(Rate) ####
		* public boolean getRateFlag获取评价是否可用标志
			* false 不能弹出评价(No pop-up comments)
			* true 可以弹出评价(You can pop up comments)
		* 实现方法具体看demo(See demo for implementation methods)
	* #### 补单(Restore order) ####
		* 补单是用户支付之后，并没有获取对应的道具而使用的方法，重新下发道具给用户。
			* Is the method that the user after paying without obtaining the corresponding prop and reissues the prop to the user.
		* 补单在用户重启手机后通过public void submitExtraData(final UserExtraData data)这个方法，触发补单的机制。
			* After the user restarts the phone, public void submitExtraData(final UserExtraData data) is used to trigger the filling mechanism.
		* submitExtraData(UserExtraData extraData) 
			* 上传游戏数据方法(Methods for uploading game data)
			* extraData->dataType = 3;这个dataType必须是3（3是代表进入游戏）
				* extraData->dataType = 3; The dataType must be 3.
			* submitExtraData方法必须在游戏的主界面调用该方法。
				* The submitExtraData method must be called from the main interface of the game.
	* #### 退出(Exit) ####
		* public void exitSDK(final ASCExitListener callback)
		* public boolean isSupportExit()
			* 这个方法是判断是否有渠道自带退出。
				* The method is to determine if there is a built-in exit channel.
			* 如果是false的话(If it's false)
				* 第一种可以使用自己的退出界面
					* The first can use its own exit screen
				* 第二种使用exitSDK方法调用默认的退出界面
					* The second method calls the default exit interface using the exitSDK method
		* 退出必须调用上传数据方法submitExtraData
			* extraData->dataType = 5;这个dataType必须是5（5是代表退出游戏）
				* extraData->dataType = 5; The dataType must be 5.

* ### 非必接功能(Non-required function) ###
	* #### OBB挂载 ####
		* public void DownloadObb(final String partID) 
			* 下载obb接口(Download the obb interface)
			* 默认使用我们自己的下载界面(By default, we use our own download interface)
		* partID 是指多个obb文件时，指定obb的下标。
			* PartID refers to the subscript of obb when multiple obb files are specified.
		* public void DownloadSuccess(final String name) 
			* 下载obb成功通知服务器(Download obb successfully notifies server)
			* 适用于自己的下载界面(For your own download screen)
	
	* #### 互推功能(Elect function) ####
		* public boolean getElectFlag(String type)
			* 获取推荐功能是否可用(Get whether the recommendation function is available)
		* public void showElect(final boolean auto, final String type)
			* 展示推荐(Shows the recommended)
			* boolean auto
				* true 自动弹出(启动游戏，或者游戏结束)
					* Auto pop-up (start game, or end game)
				* false 通过按钮点击事件触发。
					* Is triggered by the button click event.
		* type 是指弹出的类型
			* begin 游戏未通关(Game not cleared)
			* end 游戏通关(Game over)
			
					

## **广告模块(Advertising module)** ##

> 此模块适用于纯广告游戏，但是不包含内购。
>
> This module is suitable for purely commercial games, but does not include in-game purchases.

* ### 广告功能(Necessary function) ###

	* #### <font color=red >AndroidManifest.xml 配置(必接 Must be)</font> ####
		* AndroidManifest.xml文件里application下必须包含属性android:name="com.asc.sdk.ASCApplication"
			* The androidmanifest.xml file must contain attributes under application android:name="com.asc.sdk.ASCApplication"
		* 如果有第三方需要在Application里面初始化，可以继承ASCApplication，把自己继承的类填写到AndroidManifest.xml里面的android:name=“xxx.xxx.xxx.xxx.xxxApplication”属性。
			* If there is need to a third party in Application initialization, can inherit ASCApplication, his inheritance of class to fill out to AndroidManifest. The XML in the android:name=“xxx.xxx.xxx.xxx.xxxApplication” attribute.
		* 例如(For example)：<application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" 
        android:name="com.asc.sdk.ASCApplication">
	* #### ASCPlatform.java ####
		*  这个是所有游戏对接的接口类，所有的功能都是通过ASCPlatform调用对应的接口，实现交互功能。
		*  This is the interface class for all games. All functions are implemented by calling the corresponding interface of ASCPlatform.
		
	* #### <font color=red >初始化SDK(Initialize SDK) (必接 Must be)</font>####
		* public void init(Activity context, final ASCInitListener callback)
		* ASCInitListener 回调监听(The callback listener)
			* public void onInitResult(int code, String msg)
				* 初始化SDK结果回调(Initialize the SDK result callback)
			* public void onLoginResult(int code, UToken data)
				* 登陆结果回调(Login result callback)
			* public void onVideoResult(int code, String msg);
				* 视频广告回调(Watch the video AD callback)
			* public void onGiftResult(String msg);
				* 礼包兑换结果回调(Gift pack redemption results in a pullback)
		* onInitResult
			* 调用init()方法后，初始化之后会回调初始化回调方法
				* After initialization, the callback initializes the callback method
	* #### <font color=red >登陆(Login) (必接 Must be)</font>####
		* public void login(Activity context)
		* 一般我会在初始化成功之后调用login()
			* I normally call login() after I've initialized it successfully
		* onLoginResult(int code, String msg)
			* 登陆之后会调用登陆回调接口(The login callback interface is called after logging in)
			
	* #### 礼包兑换(Gift exchange) ####
		* public void exchangeGift(final String code)
		* 如果code是8位密码，不会调用礼包兑换的界面，直接服务器验证礼包码。这种情况适用于有自己的礼包兑换界面
			* If the code is 8-bit password, the interface of gift package exchange will not be called and the gift package code will be verified by the direct server. This applies to having your own gift exchange interface
		* 如果code不是8位密码，会调用礼包兑换界面，输入礼包兑换码，使用我们默认的礼包兑换界面。
			* If the code is not 8-bit password, the gift package exchange interface will be called. Enter the gift package exchange code and use our default gift package exchange interface.
		* onGiftResult(String msg)
			* 礼包兑换结果回调方法(Gift pack redemption results callback method)
			* msg是一个json的字符串，具体参数有(MSG is a json string with the following parameters)：
				* propNumber 道具数量
				* propType 道具类型
					* Golds 金币
					* Diamonds 钻石
					* Unlock 解锁
				* msg 消息
	* #### 评价(Rate) ####
		* public boolean getRateFlag获取评价是否可用标志
			* false 不能弹出评价(No pop-up comments)
			* true 可以弹出评价(You can pop up comments)
		* 实现方法具体看demo(See demo for implementation methods)
		
				
	* #### 横幅广告(Banner ads) ####
		* 横幅广告是ASCSDK自动弹出的，不需要用户调用show方法。
			* Banner ads are automatically popped up by the ASCSDK and do not require the user to call the show method.
		* public void hideBanner()
			* 用于用户隐藏banner(Used to hide the banner for the user)
		* public void showBanner()
			* 只用于用户隐藏了banner广告，再次展示时调用的方法。
				* It's only used when the user hides the banner AD and shows it again.
			
	*  #### 开屏广告(Splash ads) ####
		*  开屏广告游戏启动的时候，会自动调用开屏展示
			* When the open screen advertising game starts, it will automatically call the open screen display
		* public boolean getSplashFlag()
			* 获取开屏广告是否可用标志
				* Gets whether the logo is available for the open screen AD
		* public void showSplash()
			* 展示开屏广告(Display open screen ads)
			* 如果getSplashFlag()为true时展示开屏广告
				* Display open screen ads if getSplashFlag() is true
				
	*  #### 插屏广告(Interstitial ads) ####
		* public boolean getIntersFlag()
			* 获取插屏广告是否可用标志
				* Gets whether the logo is available for the interstitial ads
		* public void showInters(final int index)
			* index 是当前的关卡数，如果没有关卡填0
				* Is the current level number, if there is no level fill in 0
			* 展示插屏广告(Display interstitial ads)
			
	*  #### 视频广告(Video ads) ####
		* public boolean getVideoFlag()
			* 获取视频广告是否可用
				* Get whether video ads are available
		* public void showVideo()
			* 展示视频广告(Display video ads)
		* <font color=red >onVideoResult(int code, String msg)</font>
			* 视频回调接口(Video callback interface)
			* msg 是json的字符串对象(MSG is a json string object)
				* msg包含一个videoResult的key(MSG contains a videoResult key)
				* videoResult为1时表示视频播放成功，下发奖励
					* When videoResult is 1, it means that video has played successfully and the reward will be distributed
				* videoResult为0时表示视频播放不成功，不下发奖励
					* VideoResult of 0 means that video is not played successfully and no reward will be issued
	* #### 退出(Exit) ####
		* public void exitSDK(final ASCExitListener callback)
		* public boolean isSupportExit()
			* 这个方法是判断是否有渠道自带退出。
				* The method is to determine if there is a built-in exit channel.
			* 如果是false的话(If it's false)
				* 第一种可以使用自己的退出界面
					* The first can use its own exit screen
				* 第二种使用exitSDK方法调用默认的退出界面
					* The second method calls the default exit interface using the exitSDK method
		* 退出必须调用上传数据方法submitExtraData
			* extraData->dataType = 5;这个dataType必须是5（5是代表退出游戏）
				* extraData->dataType = 5; The dataType must be 5.
				
## **统计模块(Statistics module)** ##	
> 此模块适用于游戏统计，内购与广告模块都可以使用。
>
> This module is suitable for game statistics, in-app purchase and advertising module can be used.

* ### 关卡统计(Level statistics) ###
	* 关卡统计是统计某一关卡进入之后，结果为胜利，或者失败。
		* Level statistics is the statistics of a level after the entry, the result is victory, or fail.
	* 进入关卡(Starting level)
		* public void startLevel(final String index)
		* index: 对应关卡(level index)
	* 胜利关卡(Victory level)
		* public void finishLevel(final String index)
		* index: 对应关卡(level index)
	* 失败关卡(Failure levels)
		* public void failLevel(final String index)
		* index: 对应关卡(level index)
		
* ### 道具统计(Props statistics) ###

	* 道具使用统计(Item usage statistics)
		* public void userUseBoosterInfo(final String item, final int num, final double price)
			* item为事件名称(The name of the event)
			* num为使用个数(Use the number)
			* price为道具价格(Property prices)
	* 道具购买统计(Item purchase statistics)
		*  public void userBuyBoosterInfo(final String item, final int num, final double price)
			* item为事件名称(The name of the event)
			* num为购买个数(buy the number)
			* price为道具价格(Property prices)
	* 自定义事件(Custom event)
		* public void userCustomEvent(final String eventName) 
			* eventName为事件名称(The name of the event)
		* 计次事件(Would the event)
	* <font color=red >用户基本事件统计(必接 Must be)</font>
		* public void setUserInfo(final String level,final String buff,final String coin)
			* level 用户当前最高关卡
			* buff 用户当前剩余道具数量
			* coin 用户当前剩余金币数量
		** 一般我会在初始化成功之后调用setUserInfo()
			* I normally call setUserInfo() after I've initialized it successfully 

# **<font color=red >特殊说明(Special instructions)</font>**	#
<font color=blue size=4>
> 
* 该文档为通用文档，适用于Android原生，unity，cocos2dx，flash语言开发的游戏，文档主要说明了内购与广告模块的接入方式，实际的实现，以及功能请参考对应的demo项目。
	* This document is a general document, suitable for Android native, unity, cocos2dx, flash language development of the game, the document mainly explains the in-app purchase and advertising module access, the actual implementation, as well as the function, please refer to the corresponding demo project.
* 所有对接的游戏请适配全面屏，挖孔屏，水滴屏，以及适配Android P
	* All games should be compatible with full screen, hole digging screen, water drop screen and Android P
* 如果有内购与广告混合的游戏，可以把内购和广告的融合 
	* If you have a game that mixes in-app purchases with advertising, you can mix in-app purchases with advertising
* 游戏必须添加log日志打印配置(The game must add log log print configuration)
	* AndroidManifest.xml里面加入
		* <meta-data android:name="ulog.enable" android:value="true" />
		* <meta-data android:name="ulog.level" android:value="DEBUG" />
		* <meta-data android:name="ulog.local" android:value="true" />
* 游戏必须调用登陆login和上传数据方法submitExtraData方法
	* The game must call the login and upload data method submitExtraData
* 游戏Main Activity必须添加权限请求方法
	* The game Main Activity must add permission request methods
	* public void onRequestPermissionsResult(int requestCode,String[] permissions, int[] grantResults)
* 所有广告show方法必须要判断getXXXFlag对应的方法才能展示(除了banner)
	* All the AD show methods must check the corresponding method getXXXFlag to display(In addition to the banner)
* 视频观看按钮或者界面可以通过getVideoFlag来判断隐藏
	* The video watch button or interface can be hidden by getVideoFlag
</font>
										   
		 
		
			
			
	
		
			