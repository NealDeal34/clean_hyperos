
# 《小米手机系统优化对照表及说明》

更新时间：2026-10-06（补充版），适配HyperOS1、HyperOS2、HyperOS3.

本fork test on hyperos1.0.4.0.TKJCNXM/Android13,可能具有较强特异性,用于个人备忘与数据抓取,请酌情参考.

本教程含有大量注释说明,adb本身并不困难但最好使用ai工具辅助检索与具体评估后再进行实操.

原创教程地址： https://github.com/fyonecon/clean_hyperos 。

=============================

# 基础设置与安全说明：

1️⃣ 使用前备份好手机资料；

请先解锁BL或至少对线刷/ADB有基本的认识与实操;强解BL等大部分未说明步骤可参考AI进行,请保证至少有一台外置设备进行adb而非依靠shizuku搞危险操作.

⚠️卡米时可清除手机全部数据重新开始（进入卡刷机模式）：

“关机（可长按 音量上+开关键 10s中关机），音量上+开关键 长按（大概10s），Logo出来立即只松开开关键，就可以进入清除模式。”；

2️⃣ 下面系统优化的操作旨在追求极限优化,续航与类原生体验,在定制厂商停止维护后解耦定制依赖,释放设备性能并为向第三方rom转型做准备,非爱好/刚需者请勿轻易尝试.
实测优化后on REDMI K40 Gaming/Poco F3 GT 开机内存情况9.8G/12G,可压缩系统占用到3G左右,低内存设备卡顿多来源于厂商定制自启组件而非Android本身.

3️⃣ 可以在“澎湃1、澎湃2”中使用，但此前的纯miui与之后的耦合情况可能更为复杂,请勿参考.操作前请备份手机数据到“云盘”或“手机以外的硬盘”，因为xiaomi卡米变砖任何恢复途径都会先清除数据.

4️⃣如下情况会卡米，不要操作⚠️：
```
com.android.htmlviewer HTML查看器不要卸载（重启卡米），可以停用；

com.miui.packageinstaller 应用包管理器需要先安装原生版本 adb shell cmd package install-existing --user 0 com.android.packageinstaller 否则不要卸载（重启卡米）；

com.xiaomi.metoknlp 网络位置不要卸载（切换系统明暗主题时会卡米）;

com.xiaomi.location.fused 小米融合位置不要卸载（切换系统明暗主题时会卡米）;

com.miui.miwallpaper 小米壁纸（屏幕直接黑屏，任何东西都看不到）,黑屏后需要线控adb重新启动com.miui.home或第三方home同时通过原生接口重新指定壁纸,无条件不要操作;

```

5️⃣ 尽量使用Win、Mac的ADB环境（下载Win或Mac的ADB工具：
https://developer.android.google.cn/tools/releases/platform-tools?hl=zh-cn 
）并检查miflash与usb驱动的安装和适配情况，简单优化也可使用手机端ADB软件如shizuku,adbshell与黑阈等；
请检查是否已打开开发者模式的“USB调试、USB调试（安全模式）、无线调试”这3个开关.

6️⃣ 澎湃2比澎湃1好用，推荐澎湃2；澎湃3比澎湃2好用，推荐澎湃3。使用本教程从2升级到3重启时可能会卡米，建议先手动备份2文件系统到电脑里面，再升级到3；

7️⃣ 虚拟内存建议关闭或只设置为刚需值,该"功能"除了低劣的营销噱头,心理安慰与大幅度降低设备RAM寿命外无任何作用,希望尽可能延长设备寿命请保持关闭


# 常用设置：

## 1. 设置-应用-卸载所有可卸载的预装应用.

## 2. 设置负一屏，关闭广告，关闭卡片

## 3. 设置桌面，重点关闭上滑搜索、设置任务栏样式

## 4. 设置日历，关闭广告

## 5. 设置文件，关闭云盘

## 6. 设置相机，打开Live Photo，关闭位置

## 7. 设置-应用-其他应用（右上角）-关闭广告，关闭第三方应用统计

## 8. 设置-隐私-安全-关闭所有选项（各种安全检测、关闭广告、关闭网页链）

## 9. 设置-状态栏-控制中心-关闭智能设备，打开无字模式，打开网速显示，设置通知样式

## 10. 设置- Wi-Fi-网络加速-关闭网络切换

## 11. 设置-亮度与显示-刷新率设置为60Hz，字体默认大小+粗体

## 12. 设置-连续点击OS版本号，打开开发者模式-打开USB调试、动画设置为0.5倍、分辨率设置为327或339或340或350

## 13. 通讯录-关闭营业厅

## 14. 电话中拨号：

显示5G开关：
```
*#*#54638#*#*
```
显示VoLTE开关：
```
*#*#86583#*#*
```

## 15. 设置-更多设置-内存扩展-关闭

===自动重启手机===

## 16. 导入铃声、APK（🚩可选）：可通过搞机工具箱等便捷操作

## 16.1. 下载基础APK安装包：

安装常用APK Google版：
>
>Firefox浏览器：https://ftp.mozilla.org/pub/fenix/releases/
安装后开启并设置cloudfalre私有doh(参考ai进行),即可规避dns污染访问https://www.apkmirror.com 下载大部分应用的官方安装包,目前Firefox是Android中唯一同时支持扩展与doh的浏览器.
>
>部分应用安装需用到f-droid,apkpure或mt文件管理器
>
>安装常用软件如Google play,Google service,LibCheck,DevCheck,Gboard输入法等,隐藏键值设置软件SetEdit
>
>系统隐藏设置查找（Activity Launcher）： https://www.malavida.com/en/soft/activity-launcher/android/
>
>类原生桌面lawnchair： https://lawnchair.app/ （需在该App设置里手动关闭软件禁用才能设置默认桌面）
> 
> 如下，使用第三方软件+ADB来代替小米系统的对第三方桌面下的导航手势限制：
- 安装第三方手势导航软件 Ogesture（ https://github.com/tanujnotes/Ogesture ），打开该软件无障碍（先在桌面长按Ogesture的图标进入到“关于软件”-底部有三个点到按钮，打开“允许软件限制”，顺便设置电池为无限制，再打开手机设置-更多设置-无障碍-已下载应用-开启无障碍）；
- 打开Ogesture软件，开启手势导航 后 即可使用手势导航了；
- 隐藏导航栏三键:"adb shell settings put global force_fsg_nav_bar 1",或通过Setedit在global table中设置force_fsg_nav_bar值为1并勾选"perform this action on reboot".
>
> 谷歌相机： https://gcamapk.io/zh-CN/download-gcam-apk/

## 17. 用 DevCheck 设置，（用此软件的目的是调出原生安卓设置。可选。）：
```

“com.android.htmlviewer #HTML查看器”，关闭通讯录权限；

“com.miui.packageinstaller #应用包管理器”，关闭通讯录权限；

“com.android.providers.downloads #下载器”，关闭通讯录权限。

“com.xiaomi.metoknlp #网络位置”，关闭通讯录权限。

“com.xiaomi.account #小米账号”，关闭通讯录、电话权限。

“com.miui.home #系统桌面”，关闭通讯录、电话权限。

“com.xiaomi.bluetooth #小米蓝牙地图”，与系统蓝牙不是一回事，可能与生成蓝牙地图、物品找回有关。停用此app。

```

## 18.8 ⚠️设置-开发者模式-“系统优化”（os1、os2中可以直接看到，os3中连续点击“开发者模式-自动填充-重置为默认值”3次可以看到。）-关闭。
> 关闭时：
> 
> 1️⃣“app安装”变成android原生安装器；
> 
> 2️⃣字体设置可能失效或不能更改；
> 
> 3️⃣主题图标产生异常（小米自带app图标异常，反而别的应用可能是正常的，怀疑是小米故意恶心人）；
> 
> 4️⃣小米权限管理变成android原生权限管理（应用列表权限原生安卓中无此功能，而国内有，但是国内的这个应用权限功能只是个摆设和安慰剂功能）
> 
> 5️⃣应用自启设置不了，需要设置的话，可以恢复开启“小米系统优化”，设置好后再关闭“小米系统优化”；
>

（澎湃的纯净模式，近似原生安卓，此时内置的小米的安全追踪大部分关闭、系统主题部分失效<请提前设置好>、软件安装自由度火力全开、权限管理近似原生。）

## 19. 用ADB删除系统应用（⚠️⚠️⚠️“#”为暂时不要删除的应用，“#”越多，越不建议删⚠️⚠️⚠️）： 

获取当前安卓的用户标记，对多用户系统同样有用（如，0 、10 ）：
```
adb shell am get-current-user
```

### 必删App，广告与游戏：

```

adb shell pm uninstall --user 0 com.android.browser #小米浏览器

adb shell pm uninstall --user 0 com.miui.yellowpage #生活黄页(小米黄页)

adb shell pm uninstall --user 0 com.miui.hybrid #快应用服务框架(小米社区)

adb shell pm uninstall --user 0 com.miui.analytics #小米广告分析

adb shell pm uninstall --user 0 com.miui.systemAdSolution #小米系统广告解决方案

adb shell pm uninstall --user 0 com.miui.guardprovider #反诈

adb shell pm uninstall --user 0 com.miui.bugreport #用户bug反馈

adb shell pm uninstall --user 0 com.miui.securityadd #游戏加速

adb shell pm uninstall --user 0 com.xiaomi.gamecenter.sdk.service #小米游戏中心服务

adb shell pm uninstall --user 0 com.xiaomi.gamecenter #小米游戏中心（os1中可用）

adb shell pm uninstall --user 0 com.xiaomi.macro #自动连招

adb shell pm uninstall --user 0 com.miui.misightservice #系统质量检测（os3中有）

adb shell pm uninstall --user 0 com.xiaomi.security.onetrack #用户数据收集与统计

```

### 可删App，性能与墓碑机制：

写在前面:除特殊说明外大部分可见的com.miui.*字样包名的包均可禁用/卸载,但激进操作前务必解锁bl/关闭system optimization/退出小米账号 并链接pc以便adb恢复/救砖.

com.lbe.security.miui 删除后可能导致新装应用获取权限无法正常弹窗而闪退,建议保留
com.miui.systemui.devices.overlay 是overlay中唯一比较重要的一个,删除后将引发背光异常,状态栏图标错位等问题,不建议删除
过程中请连接pc并多次重启以确保不会卡米和可adb恢复.

> 联发科机器“快霸（Duraspeed）”App的使用：
> 
> 默认关闭情况：消息、后台很及时，但是对内存和电池消耗可能过快。
> 
>手动打开+app里设置白名单：特别省内存，会杀死一些垃圾进程，但是消息会延迟，FCM消息即使加了白名单也会延迟，对微信无任何影响。
> 
> 如何打开快霸app：LibCheck--搜“duraspeed”--点击duraspeed的logo--点击“设置”--“打开”。


```

adb shell pm uninstall --user 0 com.android.traceur #系统进程追踪

adb shell pm uninstall --user 0 com.xiaomi.joyose #小米性能云控

adb shell pm uninstall --user 0 com.xiaomi.mtb #鲁班MTB（系统性能调节）

adb shell pm uninstall --user 0 com.mobiletools.systemhelper #智能系统优化（含有百度地图SDK）

adb shell pm uninstall --user 0 com.miui.daemon #系统质量服务（匿名数据收集）

adb shell pm uninstall --user 0 com.miui.powerkeeper #电量和性能（智能杀后台应用，后台保活，后台保活设置。与设置里面的“电池”还不一样，设置里面的“电池”由“com.miui.securitycenter #安全服务”管理。关闭“系统优化”后，非常建议删除。）

```

### 可删App，小米服务：

```

adb shell pm uninstall --user 0 com.miui.miservice #服务反馈

adb shell pm uninstall --user 0 com.milink.service #互联互通服务

adb shell pm uninstall --user 0 com.xiaomi.mi_connect_service #小米互联互通服务

adb shell pm uninstall --user 0 com.miui.carlink #CarWith

adb shell pm uninstall --user 0 com.xiaomi.ab #小米售后支持

adb shell pm uninstall --user 0 com.miui.phrase #常用语（无卵用）

adb shell pm uninstall --user 0 com.miui.contentcatcher #应用扩展服务（也与广告有关）

#adb shell pm uninstall --user 0 com.xiaomi.payment #米币支付（含有银联SDK）

adb shell pm uninstall --user 0 com.unionpay.tsmservice.mi #银联统计

adb shell pm uninstall --user 0 com.wapi.wapicertmanager #WAPI证书

adb shell pm uninstall --user 0 com.miui.thirdappassistant #第三方应用分析（主要针对32位应用）

adb shell pm uninstall --user 0 com.android.adservices.api #谷歌安卓系统广告隐私保护

adb shell pm uninstall --user 0 com.miui.mishare.connectivity #小米分享（废物应用）

#adb shell pm uninstall --user 0 com.miui.nextpay #卡包网页组件（小米钱包、NFC管理等）

adb shell pm uninstall --user 0 com.android.healthconnect.controller #健康数据共享

adb shell pm uninstall --user 0 com.google.android.configupdater #（可能与小米系统权限恢复有关）

#adb shell pm uninstall --user 0 com.miui.touchassistant #悬浮球

#adb shell pm uninstall --user 0 com.miui.contentextension #传送门（识屏）

#adb shell pm uninstall --user 0 com.miui.accessibility #小米闻声

#adb shell pm uninstall --user 0 com.miui.mediaviewer #媒体查看器(MXPlayer代替)

adb shell pm uninstall --user 0 com.miui.greenguard #家人守护、系统未成年模式

adb shell pm uninstall --user 0 com.xiaomi.bluetooth #小米额外的蓝牙服务（不是安卓蓝牙。可能与附近手机设备、附近穿戴设备有关。）

#adb shell pm uninstall --user 0 com.xiaomi.digitalkey #小米数字车钥匙

#adb shell pm uninstall --user 0 com.xiaomi.continuity.sdkapp #继续服务（协同播放、投屏？）

adb shell pm uninstall --user 0 com.miui.wmsvc #似乎与快应用的信息服务有关

#adb shell pm uninstall --user 0 com.miui.autoui.ext #？

#adb shell pm uninstall --user 0 com.mi.AutoTest #售后测试

#############adb shell pm uninstall --user 0 com.tencent.soter.soterserver #微信指纹支付

#adb shell pm uninstall --user 0 com.miui.tsmclient #小米智能卡

```

### 可删App，增值服务：

```

#############adb shell pm uninstall --user 0 com.lbe.security.miui #MIUI权限管理（可用安卓自带权限管理代替。默认国内权限全开，原生安卓权限需要手动设置。）

#############adb shell pm uninstall --user 0 com.miui.securitycenter #安全服务（无界面，电池管理、应用列表、隐藏应用、应用联网、手机改名、OAID<删除securitycenter后App获取OAID将为空>、应用锁、应用杀毒、地震预警，请用“LibCheck https://www.downkuai.com/android/147630.html ”软件代替管理应用列表。如果“关闭小米手机系统优化”，强烈建议删除此App，以获得接近原声安卓的体验。此app与系统及的“全屏红色倒计时弹窗”提醒有关，卸载后提醒变成无全屏弹窗倒计时的小蓝色弹窗。此外，app与取证有关（未证实）。分身空间（不是多用户空间）删除后重启手机会自动恢复。）

#############adb shell pm uninstall --user 0 com.xiaomi.xmsf #⚠️小米服务框架（微信消息、APP后台消息推送（可与Google FCM共存）、MiPush、设备统计相关、用户大数据分析等）

#############adb shell pm uninstall --user 0 com.xiaomi.xmsfkeeper #⚠️小米服务框架Keeper

############adb shell pm uninstall --user 0 com.miui.securitycore #⚠️系统功能服务组件（应用双开、手机分身、系统多开、边缘触控）

#adb shell pm uninstall --user 0 com.miui.core #MIUI SDK（自动安装系统软件、SDK等）

#adb shell pm uninstall --user 0 com.miui.core.internal.services #（绑定在Android系统中，实际用途未找到未明确）

adb shell pm uninstall --user 0 com.miui.securitycenter.securitycenter_phone_overlay.config.overlay #(不知道)

#############adb shell pm uninstall --user 0 com.android.mms #短信（谷歌短信App代替）

#############adb shell pm uninstall --user 0 com.android.contacts #电话、通讯录、营业厅（谷歌电话App、联系人App代替。os1、os2中可删，os3中请保留。）

#############adb shell pm uninstall --user 0 com.google.android.webview #Webview(可能含有网址访问认证，Google Play里面下别的版本的webview代替)

#############adb shell pm uninstall --user 0 com.android.thememanager #系统主题(设置)（主题、图标、壁纸设置好了以后再删除此App，或者使用第三方相册设置壁纸。）

#############adb shell pm uninstall --user 0 com.android.updater #小米系统更新（需要与“com.android.providers.downloads”下载器搭配才能更新系统）

adb shell pm uninstall --user 0 com.android.quicksearchbox #桌面搜索

#######adb shell pm uninstall --user 0 com.miui.personalassistant #桌面负一屏（小米小部件、安卓小部件、负一平智能助理）

##########################adb shell pm uninstall --user 0 com.miui.home #⚠️小米桌面（可以用第三方桌面<如 https://lawnchair.app/ >替代。但是“最近任务、导航栏”消失）

```

### 可删App， 小爱同学（人工智障，按需卸载）：

```

#############adb shell pm uninstall --user 0 com.miui.voicetrigger #语音唤醒小爱

#############adb shell pm uninstall --user 0 com.miui.voiceassist #小爱同学、超级小爱

adb shell pm uninstall --user 0 com.xiaomi.aicr #MiAI引擎、澎湃AI引擎

adb shell pm uninstall --user 0 com.xiaomi.aireco #小爱建议

adb shell pm uninstall --user 0 com.xiaomi.aiasst.vision #小爱翻译

adb shell pm uninstall --user 0 com.xiaomi.scanner #小爱视觉（还需要在小爱视觉设置-关于-关闭“上传图片图片识别优化”）

adb shell pm uninstall --user 0 com.xiaomi.aiasst.service #小爱通话

```

### 可删App，小米账号与云服务（谨慎卸载）

```

adb shell pm uninstall --user 0 com.miui.cloudbackup #小米云备份

adb shell pm uninstall --user 0 com.miui.cloudservice #小米云服务（查找设备<仅能中国大陆ID>、同步“相册、蓝牙、笔记、录音”，此app可能与小米账号联动上报用户串联信息。可以用GooglePhoto、GoogleKeep、OneDrive代替）
 
#adb shell pm uninstall --user 0 com.xiaomi.micloud.sdk #小米云服务SDK

adb shell pm uninstall --user 0 com.miui.micloudsync #小米云同步（仅同步“联系人、通话、短信、安全服务、Wi-Fi”，可以用Google服务代替。）

adb shell pm uninstall --user 0 com.miui.backup #备份（只能备份有备案的App的资料，不能备份国外或者未备案App资料）

#############adb shell pm uninstall --user 0 com.xiaomi.account #小米账号（绑定任意国家ID：防止手机刷机，启用小爱同学，启用小米运动，退出小米账号后可以删除）

```

===手动重启手机===

## 20. 给手机联网：
防止手机无法使用（参考本教程的“16.1”步骤）：
- 安装一个浏览器，比如Edge、Firefox；
- 安装一个输入法，比如GBoard.
- 安装一个第三方桌面.

## 21. 在小米应用商店下载“Google Play”。
安装完成后卸载掉小米应用商店（关闭广告相关开关以后还会推送广告，必须卸载）：
```
#adb shell pm uninstall --user 0 com.sohu.inputmethod.sogou.xiaomi #搜狗输入法（微信输入法、GBoard输入法代替）

#adb shell pm uninstall --user 0 com.xiaomi.market #小米应用商店，请在此下载安装Google Play（别的商店代替）、Google Play）

#adb shell pm uninstall --user 0 com.android.providers.downloads #下载器（下载主题、下载系统、自动下载和安装App。如果需要更新系统，ADB恢复下载器软件+重启手机。）

```

## 22. 更改密码填充服务。
设置-更多设置-语言与输入法-密码和帐号-首选服务-选择小米账号或者Google或Edge浏览器或Auth认证App（这是密码自动填充服务）

===（手动重启手机）===


## 23. 必备扩展：
- 登录微信（Play版微信需要下载小程序扩展）；
- Firefox 添加ad扩展（国内IP不可用此功能）；
- 三星浏览器 添加扩展（在Google Play里下载扩展），如果无法安装，清除 浏览器 数据并重新设置。
  - 国内环境如何安装三星浏览器插件：
      - 在浏览器地址栏输入：internet://debug/
      - 此时在点击浏览器设置，翻到最底下就会出现一个Debug settings选项，点击进去
      - 找到选项：Feature variation test，点击进入 。点击Sales code选项，翻到最底下点击Other，输入：TGY，确认。 点击Country code选项，翻到最底下点击Other，输入：Hong Kong，确认。 点击Country iso code选项，翻到最底下点击Other，输入：HK，确认。
      - 关闭三星浏览器，去任务卡片中划掉它，其实就是重启一下。
      - 然后点击设置中的广告拦截，此时就可以点击下载拦截器了。如果下载不了（新版本需要在Google Play里面下载插件），就用直接安装广告拦截软件的APK的方法（Samsungbrowser V30.0.0 已验证）：
          - AdGuard for SamsungBrowser： https://github.com/fyonecon/clean_hyperos/releases/download/HyperOS3-20260428/AdGuard_samsung.browser_2.8.0.apk.7z （需要把“.7z”后缀直接删掉即可是对应.apk文件。）
          - ABP for SamsungBrowser：https://github.com/fyonecon/clean_hyperos/releases/download/HyperOS3-20260428/ABP_samsung.browser_2.5.7.apk.7z （需要把“.7z”后缀直接删掉即可是对应.apk文件。）

## 24. Firefox、三星浏览器、Edge 添加vivo H5应用商店（ https://h5.appstore.vivo.com.cn ）到桌面快捷方式。。国内安卓应用商店的的访问规则太不稳定了，刚用没多久，网页访问就废了。。此条暂时作废。。

## 25. 这里做一个关于“国内 OAID”和“Google ADID”的说明：
- 这两个ID都可以跨App追踪用户，实现用户“隐私与广告”跨App互通。
- 删除OAID：adb删除“com.miui.securitycenter”即可永久关闭OAID。
- 删除Google ADID：在“Google Services”软件里找“Ads”，选择“删除”即可永久关闭ADID。
- 设备指纹ID：无需关心。
- App列表权限：这个不管你优不优化国内任何安卓系统，App都能在不需要用户同意的情况下获取到App列表，所以无需在意，除非你用国外安卓系统。微信、京东、拼多多没有获取应用列表权限，其他App中90%都在暗中获取应用列表权限。

⚠️ App OAID及ADID值检查：https://github.com/fyonecon/clean_hyperos/releases/download/HyperOS3-20260428/OAID.apk.7z （需要把“.7z”后缀直接删掉即可是对应.apk文件。）

⚠️ App列表权限检查：https://github.com/fyonecon/clean_hyperos/releases/download/HyperOS3-20260428/AppListViewer.apk.7z （需要把“.7z”后缀直接删掉即可是对应.apk文件。）



## 26. 提升手机系统流畅度说明：
- 关闭“系统优化”后，系统的动画和页面切换效果将变为安卓原生的，即使是60Hz，也比“小米自带动画特效”流畅很多，特别是在低端机上表现特别明显。
- 小米桌面App真的卡，一个桌面App有可能占800MB的ram，特别是在多系统用户空间下特别卡。我使用第三方桌面lawnchair https://lawnchair.app/ （需在该App设置里手动关闭软件禁用才能设置默认桌面），虽然牺牲了手势操作（启用第三方桌面小米手势就会用不了，只能用三大键，这个真的很鸡贼），但是换来了操作流畅。

## 27. 如何更新手机系统自带Webview：
- 如果你直接打开Google Play来更新Webview，除了安装 Webview Dev 版这个办法，是不能直接更新自系统带版的，所以只能按照上面步骤曲线更新Webview.
- 使用浏览器搜索"Android systemwebview google"并打开网页版Googleplay再跳转到应用内即可,或者直接在apkmirror下载安装包后安装,大部分不可见应用都可参考此步骤.

## 29. 开启Shizuku：
- 安装 Shizuku（ https://github.com/RikkaApps/Shizuku/releases ）；
- 登录小米账号com.xiaomi.account + 有安全服务com.miui.securitycenter软件；
- 重启手机；
- 在开发者模式的“USB调试、USB调试（安全模式）、无线调试”这3个按钮；
- 打开 Shizuku 按照步骤完成授权。

## 29. 最后，如果玩机玩累了，请转到其他家的手机。手机里面的小心思，就这样吧。

=============================

# 恢复已删除App、系统数据清除等：

1️⃣ 恢复已删除的预装应用举例：

adb shell cmd package install-existing （应用包名）

```

adb shell cmd package install-existing com.xiaomi.market #小米应用商店

adb shell cmd package install-existing com.android.providers.downloads #下载管理器

adb shell cmd package install-existing com.android.updater #小米系统更新

adb shell cmd package install-existing com.lbe.security.miui #MIUI权限管理（可用安卓自带权限管理代替）

adb shell cmd package install-existing com.miui.securitycenter #安全服务（无界面、应电池、应用列表、隐藏应用、应用联网、手机名+Wi-Fi名+蓝牙名更改校验）

adb shell cmd package install-existing com.android.thememanager #系统主题(设置)

adb shell cmd package install-existing com.android.mms #短信

adb shell cmd package install-existing com.android.contacts #电话和通讯录

adb shell cmd package install-existing com.miui.personalassistant #桌面负一屏(小部件、智能助理)

adb shell cmd package install-existing com.miui.securitycore #系统功能服务组件（应用双开、手机分身、边缘触控）

adb shell cmd package install-existing com.miui.cloudservice #小米云服务

adb shell cmd package install-existing com.xiaomi.account #小米账号

adb shell cmd package install-existing com.miui.micloudsync #小米云同步

adb shell cmd package install-existing com.xiaomi.xmsf #小米服务1（微信消息、Mipush）

adb shell cmd package install-existing com.xiaomi.xmsfkeeper #小米服务2（微信消息、Mipush）

adb shell cmd package install-existing com.miui.powerkeeper #电量和性能（智能杀后台应用，后台保活相关）

adb shell cmd package install-existing com.android.camera #小米相机

adb shell cmd package install-existing com.miui.core #（系统SDK API）

adb shell cmd package install-existing com.miui.home #小米桌面



```

禁用和恢复禁用的命令：
```
禁用：
adb shell pm disable-user --user 0 [应用包名]

恢复禁用：
adb shell pm enable [应用包名]

```

禁止后台联网的命令（可能无用）：
```
禁用：
adb shell cmd netpolicy add restrict-background [应用包名]

验证：
adb shell cmd netpolicy list restrict-background

恢复：
adb shell cmd netpolicy remove restrict-background [应用包名]

```

2️⃣ 给手机导入导出文件（🚩示例）：

将手机系统备份文件夹导入电脑中：
adb pull /sdcard/MIUI/backup/AllBackup ../android_backup/

将电脑文件夹导入手机系统恢复文件夹中：
adb push ../android_backup/AllBackup /sdcard/MIUI/backup/

给手机导入铃声：
adb push ../android_backup/Files/铃声.zip /sdcard/Documents/

给手机导入APK：
adb push ../android_backup/Files/APK.zip /sdcard/Documents/

给手机导入卡刷包：
adb push ../android_backup/OS/RedmiNote14Pro-CN-malachite-ota_full-OS2.0.10.0.VOOCNXM-user-15.0-13718ddde6.zip /sdcard/Documents/ 

3️⃣ 多用户系统（系统隔离度高）：

什么是“多用户”：设置-其他设置-用户-（多用户：包括访客用户）。

什么是“分身用户”：设置-密码安全-手机分身-（分身用户）。

“分身用户”的SecurityCore删除后会自动恢复，而“多用户”不会。

“关闭手机系统优化”后，“分身用户”可以自由安装软件，“多用户”不行（多用户只能通过Play商店、小米商店安装程序）。

如何设置“多用户”锁屏密码：无法直接修改密码，不管系统收否关闭或开启系统优化，都无法直接设置锁屏密码，但是可以曲线设置：进入目标用户系统-设置-主题-指纹识别样式-（设置锁屏密码）。

注意，锁屏密码设置后就不可以更改了，此处你要是设置了指纹，那么这个指纹可以解锁主用户的锁屏，请不要设置多用户的指纹解锁，如果设置了，可以在主用户的“密码与安全”将这个指纹删除。

如何设置“分身用户”锁屏密码：：设置-密码安全-手机分身-（管理）

⚠️“分身用户”在使用几天后可能遇到锁屏密码失效的问题（会彻底丢失资料）。建议使用“Multi User 多用户”，极不建议使用“分身用户”。


============================
