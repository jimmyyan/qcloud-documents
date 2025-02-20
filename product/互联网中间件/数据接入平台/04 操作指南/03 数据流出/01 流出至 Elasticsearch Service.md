## 操作场景

数据接入平台 DIP 提供数据流出能力，您可以将 CKafka 数据分发至 Elasticsearch Service（ES）便于海量数据存储搜索、实时日志分析等操作。
>?只支持7.0以上版本的 Elasticsearch Service。

## 前提条件

该功能目前依赖 Elasticsearch Service 服务，使用时需开通相关产品功能。

## 操作步骤

### 创建数据流出任务

1. 登录 [DIP 控制台](https://console.cloud.tencent.com/ckafka/datahub-overview)。
2. 在左侧导航栏单击**数据流出**，选择好地域后，单击**新建任务**。
3. 目标类型选择 **Elasticsearch Service**，单击**下一步**。
   ![](https://qcloudimg.tencent-cloud.cn/raw/9be51dee0aa418113302d7ec42c800a9.png)
   - 任务名称：只能包含字母、数字、下划线、"-"、"."。
   - CKafka 实例：选择数据源 CKafka。
   - 源 Topic：选择源 Topic。
   - 源数据：支持拉取源数据。
   - 起始位置：转储时历史消息的处理方式，topic offset 设置。
   - 实例集群：选取腾讯云 Elasticsearch Service 实例集群信息。
   - 实例用户名：输入 Elasticsearch 实例用户名，腾讯云 Elasticsearch 默认用户名为 elastic，且不可更改。
   - 实例密码：输入 Elasticsearch 实例密码。
   - 丢弃解析失败消息：消息解析失败原因一般是转储到 ES 的消息超过了32kb 或者消息与目标 index 的 schema 不一致。若不丢弃解析失败消息，则任务异常，转储不再继续。
4. 单击**提交**，完成任务创建。



### 编辑数据源和数据目标

1. 登录 [DIP 控制台](https://console.cloud.tencent.com/ckafka/datahub-overview)。
2. 在左侧导航栏单击**数据流出**，单击目标任务的**ID**，进入任务基本信息页面。
3. 单击**数据源**模块右上角的**更改数据源**，修改数据源信息。
4. 单击**数据目标**模块右上角的**更改数据目标**，修改数据目标信息。



### 暂停任务

在 **[数据流出](https://console.cloud.tencent.com/ckafka/datahub-sink)** 页面，单击目标任务的操作栏的**更多** > **暂停任务**，二次确认后可暂停任务。

> ?当您发现数据流出任务影响了 CKafka 正常服务时，可以暂停数据流出。

### 启动任务

在 **[数据流出](https://console.cloud.tencent.com/ckafka/datahub-sink)** 页面，单击目标任务的操作栏的**更多** >**启动任务**，二次确认后可将暂停任务恢复。

>?处于暂停状态的任务可以重新启动，将继续转储数据。

### 重启任务

在 **[数据流出](https://console.cloud.tencent.com/ckafka/datahub-sink)** 页面，单击目标任务的操作栏的**更多** >**重启任务**，二次确认后可以重新开始任务。

> ?任务在**异常**状态时，可以重启任务，重启任务表示重新开始任务，不会影响到已经转储的数据和相关的 CKafka 实例。

### 重建任务

创建失败的任务可能是因为创建任务时的配置失误，用户可以手动重建任务。

1. 在 **[数据流出](https://console.cloud.tencent.com/ckafka/datahub-sink)** 页面，单击目标任务的操作栏的**更多** >**重建任务**，进入任务设置页面。
2. 指定新的任务名称并编辑数据源后，单击**提交**，完成任务重建。

### 复制任务

当您有大量配置相似的任务时，在第一个任务创建成功后可以通过复制任务功能将任务进行复制。

1. 在 **[数据流出](https://console.cloud.tencent.com/ckafka/datahub-sink)** 页面，单击目标任务的操作栏的**更多** >**复制任务**，进入任务设置页面。
2. 指定新的任务名称并编辑数据源后，单击**提交**，完成任务重建。

### 删除任务

在  **[数据流出](https://console.cloud.tencent.com/ckafka/datahub-sink)** 页面，单击目标任务的操作栏的**删除**，在二次确认弹窗中单击**确认**，可删除任务。

> ?
>
> - 删除任务表示停止数据转储并删除任务记录，不会影响到已经转储的数据和相关的 CKafka 实例。
> - 任务一旦删除不可恢复，请您谨慎操作。



### 查看监控

1. 在  **[数据流出](https://console.cloud.tencent.com/ckafka/datahub-sink)** 页面，单击目标任务的 **ID**，进入任务基本信息页面。
2. 在任务详情页顶部，单击**消费进度**页签。
3. 选择**消费组**页签，可以查看消费组的消费进度
   ![](https://qcloudimg.tencent-cloud.cn/raw/6372df04909894e93d50eeaa0e475890.png)
   单击**查看消费者详情**可以看到消费者详细信息。
![](https://qcloudimg.tencent-cloud.cn/raw/010d46e0d0d75a9fe73a17f55dc34cdd.png)
4. 选择**监控**页签，选择要查看资源，设置好时间范围，可以查看对应的监控数据。
   <table>
       <tr>
           <th>图标</th>
           <th>说明</th>
       </tr>
       <tr>
           <td><img src ="https://main.qcloudimg.com/raw/9ba57bbd3b8ef3efc4f687d63d27a46d.png" style ="margin:0"></td>
           <td>单击可查看监控指标同环比。</td>
       </tr>
       <tr>
           <td><img src ="https://main.qcloudimg.com/raw/34bdbdbdabb7b5720bf17d78c636a4ad.png" style ="margin:0"></td>
           <td>单击可刷新获取最新的监控数据。</td>
       </tr>
       <tr>
           <td><img src ="https://main.qcloudimg.com/raw/8f2bf7f4df9ddd959f0ecb69fdda8e4c.png" style ="margin:0"></td>
           <td>单击可将图表复制到 Dashboard，关于 Dashboard 请参见 <a href="https://cloud.tencent.com/document/product/248/47161">什么是 Dashboard</a>。</td>
       </tr>
       <tr>
           <td><img src ="https://main.qcloudimg.com/raw/af20129df7be46f33ab7d3598f6e9213.png" style ="margin:0"></td>
           <td>勾选后可在图表上显示图例信息。</td>
       </tr>
   </table>


![](https://qcloudimg.tencent-cloud.cn/raw/9cd9d5da4bc8eaafd1c471b7ab2c58f9.png)



### 查看消息

1. 登录 [DIP 控制台](https://console.cloud.tencent.com/ckafka/datahub-overview)。
2. 在左侧导航栏单击**数据流出**，单击目标任务的**ID**，进入任务基本信息页面。
3. 单击**查看消息**页签，选择好 Topic 和分区后，可以查看数据流出成功的最近5条、20条、60条和100条消息。
   ![](https://qcloudimg.tencent-cloud.cn/raw/171fd12cb463579aec77a1702cbcf988.png)






## 产品限制和费用计算

转储速度与 CKafka 实例峰值带宽上限有关，如出现消费速度过慢，请检查 CKafka 实例的峰值带宽。

  
