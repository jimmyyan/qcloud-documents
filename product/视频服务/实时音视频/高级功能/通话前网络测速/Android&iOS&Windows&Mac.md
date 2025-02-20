
普通用户很难评估网络质量，建议您在进行视频通话之前先进行网络测试，通过测速可以更直观地评估网络质量。
  
## 注意事项

- 视频通话期间请勿测试，以免影响通话质量。
- 测速本身会消耗一定的流量，从而产生极少量额外的流量费用（基本可以忽略）。

## 支持的平台

| iOS | Android | Mac OS | Windows | Electron|微信小程序 | Web 端|
|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|
| &#10003; |  &#10003; | &#10003; | &#10003; | &#10003;    | ×  | &#10003;（参考：[Web 端教程](https://web.sdk.qcloud.com/trtc/webrtc/doc/zh-cn/tutorial-24-advanced-network-quality.html)）  |

## 测速的原理

![](https://main.qcloudimg.com/raw/fda70080f1d1267452ebc721e83e1392.png)

- 测速的原理是 SDK 向服务器节点发送一批探测包，然后统计回包的质量，并将测速的结果通过回调接口通知出来。
- 测速的结果将会用于优化 SDK 接下来的服务器选择策略，因此推荐您在用户首次通话前先进行一次测速，这将有助于我们选择最佳的服务器。同时，如果测试结果非常不理想，您可以通过醒目的 UI 提示用户选择更好的网络。
- 测速的结果（[TRTCSpeedTestResult](https://liteav.sdk.qcloud.com/doc/api/zh-cn/group__TRTCCloudDef__ios.html#interfaceTRTCSpeedTestResult)）包含如下几个字段：
<table>
<tr><th>字段</th><th>含义</th><th>含义说明</th>
</tr><tr>
<td>success</td>
<td>是否成功</td>
<td>本次测试是否成功</td>
</tr><tr>
<td>errMsg</td>
<td>错误信息</td>
<td>带宽测试的详细错误信息</td>
</tr><tr>
<td>ip</td>
<td>服务器 IP</td>
<td>测速服务器的 IP</td>
</tr><tr>
<td><a href="https://liteav.sdk.qcloud.com/doc/api/zh-cn/group__TRTCCloudDef__ios.html#ga25f9ccb045890cb18a5f647ef3c1f974">quality</a></td>
<td>网络质量评分</td>
<td>通过评估算法测算出的网络质量，loss 越低，rtt 越小，得分也就越高</td>
</tr><tr>
<td>upLostRate</td>
<td>上行丢包率</td>
<td>范围是[0 - 1.0]，例如0.3代表每向服务器发送10个数据包，可能有3个会在中途丢失</td>
</tr><tr>
<td>downLostRate</td>
<td>下行丢包率</td>
<td>范围是[0 - 1.0]，例如0.2代表从服务器每收取10个数据包，可能有2个会在中途丢失</td>
</tr><tr>
<td>rtt</td>
<td>网络延时</td>
<td>代表 SDK 跟服务器一来一回之间所消耗的时间，这个值越小越好，正常数值在 10ms - 100ms 之间</td>
</tr><tr>
<td>availableUpBandwidth</td>
<td>上行带宽</td>
<td>预测的上行带宽，单位为kbps， -1表示无效值</td>
</tr><tr>
<td>availableDownBandwidth</td>
<td>下行带宽</td>
<td>预测的下行带宽，单位为kbps， -1表示无效值</td>
</tr></table>

## 如何测速

通过 TRTCCloud 的 `startSpeedTest` 功能可以启动测速功能，测速的结果会通过回调函数返回。

<dx-codeblock>
::: Objective-C ObjectiveC
// 启动网络测速的示例代码, 需要 sdkAppId 和 UserSig，(获取方式参考基本功能)
// 这里以登录后开始测试为例
- (void)onLogin:(NSString *)userId userSig:(NSString *)userSid 
{
    TRTCSpeedTestParams *params;
    // sdkAppID 为控制台中获取的实际应用的 AppID
    params.sdkAppID = sdkAppId;
    params.userID = userId;
    params.userSig = userSig;
    // 预期的上行带宽（kbps，取值范围： 10 ～ 5000，为 0 时不测试）
    params.expectedUpBandwidth = 5000;
    // 预期的下行带宽（kbps，取值范围： 10 ～ 5000，为 0 时不测试）
    params.expectedDownBandwidth = 5000;
    [trtcCloud startSpeedTest:params];
}
- (void)onSpeedTestResult:(TRTCSpeedTestResult *)result {
    // 测速完成后，会回调出测速结果
}
:::
::: Java Java
//启动网络测速的示例代码, 需要 sdkAppId 和 UserSig，(获取方式参考基本功能)
// 这里以登录后开始测试为例
public void onLogin(String userId, String userSig) 
{
  TRTCCloudDef.TRTCSpeedTestParams params = new TRTCCloudDef.TRTCSpeedTestParams();
  params.sdkAppId = GenerateTestUserSig.SDKAPPID;
  params.userId = mEtUserId.getText().toString();
  params.userSig = GenerateTestUserSig.genTestUserSig(params.userId);
  params.expectedUpBandwidth = Integer.parseInt(expectUpBandwidthStr);
  params.expectedDownBandwidth = Integer.parseInt(expectDownBandwidthStr);
	// sdkAppID 为控制台中获取的实际应用的 AppID
    trtcCloud.startSpeedTest(params);
}

// 监听测速结果，继承 TRTCCloudListener  并实现如下方法
void onSpeedTestResult(TRTCCloudDef.TRTCSpeedTestResult result)
{
    // 测速完成后，会回调出测速结果
}
:::
::: C++ C++
// 启动网络测速的示例代码, 需要 sdkAppId 和 UserSig，(获取方式参考基本功能)
// 这里以登录后开始测试为例
void onLogin(const char* userId, const char* userSig)
{
    TRTCSpeedTestParams params;
    // sdkAppID 为控制台中获取的实际应用的 AppID
    params.sdkAppID = sdkAppId;
    params.userId = userid;
    param.userSig = userSig;
    // 预期的上行带宽（kbps，取值范围： 10 ～ 5000，为 0 时不测试）
    param.expectedUpBandwidth = 5000;
    // 预期的下行带宽（kbps，取值范围： 10 ～ 5000，为 0 时不测试）
    param.expectedDownBandwidth = 5000;
    trtcCloud->startSpeedTest(params);
}

// 监听测速结果
void TRTCCloudCallbackImpl::onSpeedTestResult(
             const TRTCSpeedTestResult& result)
{
    // 测速完成后，会回调出测速结果
}
:::
::: C# C#
// 启动网络测速的示例代码, 需要 sdkAppId 和 UserSig，(获取方式参考基本功能)
// 这里以登录后开始测试为例
private void onLogin(string userId, string userSig)
{
    TRTCSpeedTestParams params;
    // sdkAppID 为控制台中获取的实际应用的 AppID
    params.sdkAppID = sdkAppId;
    params.userId = userid;
    param.userSig = userSig;
    // 预期的上行带宽（kbps，取值范围： 10 ～ 5000，为 0 时不测试）
    param.expectedUpBandwidth = 5000;
    // 预期的下行带宽（kbps，取值范围： 10 ～ 5000，为 0 时不测试）
    param.expectedDownBandwidth = 5000;
    mTRTCCloud.startSpeedTest(params);
}

// 监听测速结果
public void onSpeedTestResult(TRTCSpeedTestResult result)
{
    // 测速完成后，会回调出测速结果
}
:::
</dx-codeblock>

## 测速工具
如果您不想通过调用接口的方式来进行网络测速，TRTC 还提供了桌面端的网络测速工具程序，帮助您快速获取详细的网络质量信息。
### 下载链接
[Mac](https://liteav.sdk.qcloud.com/customer/%E6%B5%8B%E8%AF%95/%E6%B5%8B%E8%AF%95%E5%B7%A5%E5%85%B7/trtc-network-tools-latest.dmg) ｜[Windows](https://liteav.sdk.qcloud.com/customer/%E6%B5%8B%E8%AF%95/%E6%B5%8B%E8%AF%95%E5%B7%A5%E5%85%B7/trtc-network-tools_Setup_latest.exe)
### 测试指标
                        
| 指标         | 含义                                                                                                               |
|--------------|--------------------------------------------------------------------------------------------------------------------|
| WiFi Quality | Wi-Fi 信号质量                                                                                                  |
| DNS RTT      | 腾讯云的测速域名解析耗时                                                                          |
| MTR          | MTR 是一款网络测试工具，能探测客户端到 TRTC 节点的丢包率与延时，还可以查看路由中每一跳的具体信息 |
| UDP Loss     | 客户端到 TRTC 节点的 UDP 丢包率                                                                                        |
| UDP RTT      | 客户端到 TRTC 节点的 UDP 延时                                                                                        |
| Local RTT    | 客户端到本地网关的延时                                                                                        |
| Upload       | 上行预估带宽                                                                                                       |
| Download     | 下行预估带宽                                                                                                       |


### 工具截图
登录：
![登录](https://qcloudimg.tencent-cloud.cn/raw/79417170a3679db66667075cc5bd4d6e.png)
快速测试：
![](https://qcloudimg.tencent-cloud.cn/raw/e1d7e4b4bfcd1c7c3916e56095b8ba24.png)
持续测试：
![](https://qcloudimg.tencent-cloud.cn/raw/7b03e1ef8c940ef255b912b8b1fa7b49.png)



