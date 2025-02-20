本文主要介绍如何快速将腾讯云即时通信 IM SDK 集成到您的 Unitiy 项目中。

## 环境要求

| 平台    | 版本                                                         |
| ------- | ------------------------------------------------------------ |
| Unity   | 2019.4.15f1 及以上版本。                                     |
| Android | Android Studio 3.5及以上版本，App 要求 Android 4.1及以上版本设备。 |
| iOS     | Xcode 11.0及以上版本，请确保您的项目已设置有效的开发者签名。 |

## UPM 集成（推荐）
1. 修改 manifest.json 文件：
![](https://qcloudimg.tencent-cloud.cn/raw/88597bf131303e1b444baa33c641924e.png)

2. 修改如下：
```json
{
    "dependencies":{
    "com.tencent.imsdk.unity":"https://github.com/TencentCloud/TIMSDK.git#unity" 
  }
}
```

3. 在 Unity Editor 中打开项目，等候依赖加载完毕，确认Tencent Cloud IM 已经加载完成。
![img](https://qcloudimg.tencent-cloud.cn/raw/d98dfb17bbee6c0319e370de6f2ba9dd.jpg)

4. 该步骤为测试环节，您可 [下载测试脚本](https://github.com/TencentCloud/TIMSDK/blob/master/Unity/im_unity_sdk_plus/Assets/Demo/TestApi.cs)，将文件解压后，放入项目中，并绑定 TestApi.cs 到任意场景上。
![img](https://qcloudimg.tencent-cloud.cn/raw/b4d770775523fdd76b75f1d80f07c925.jpg)
![img](https://qcloudimg.tencent-cloud.cn/raw/940da8044cd80db27d08a7b0dff45b94.png)

