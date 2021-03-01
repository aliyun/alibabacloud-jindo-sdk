# Jindo DistCp使用云监控服务进行失败告警

Jindo DistCp（分布式文件拷贝工具）是用于大规模集群内部和集群之间拷贝文件的工具。使用MapReduce实现文件分发，错误处理和恢复，把文件和目录的列表作为map/reduce任务的输入，每个任务会完成源列表中部分文件的拷贝。

云监控服务（简称CMS），可用于收集阿里云资源的监控指标或用户自定义的监控指标，探测服务可用性，以及针对指标设置警报。使您全面了解阿里云上资源的使用情况和业务运行状况，并及时对故障资源进行处理，保证业务正常运行。

您可以指定本次Jindo DistCp任务结束后是否需要开启CMS，以上报任务失败信息，并使用CMS控制台来配置告警功能。

## 步骤

### 1. 创建报警联系人和报警联系组

具体请参考文档[《创建报警联系人或报警联系组》](https://help.aliyun.com/document_detail/104004.html?spm=a2c4g.11186623.6.672.1a493b70h9Bgby)

### 2. 创建应用分组

具体请参考文档[《创建应用分组》](https://help.aliyun.com/document_detail/45243.html?spm=a2c4g.11186623.6.573.469f3142Ps85rh)

创建**标准应用分组**，获取分组ID，即cmsGroupId

### 3. 创建报警规则

具体请参考文档[《创建报警规则》](https://help.aliyun.com/document_detail/103171.html?spm=a2c4g.11186623.6.663.18f87d95abSz2u)

**事件类型**选择**自定义事件**，并指定事件名称为: **JindoDistcpCounter**

### 4. 配置环境变量

| 环境变量 | 说明 |
| --- | --- |
| cmsAccessKeyId | 设置账号对应AccessKey ID |
| cmsAccessSecret | 设置账号对应Access Key Secret |
| cmsRegion | 设置CMS所在Region，需要与其他相关的云上资源一致，例如ECS，OSS，EMR所对应的Region |
| cmsGroupId | 设置创建的CMS应用分组ID |

示例如下

```bash
export cmsAccessKeyId=<your_key_id>
export cmsAccessSecret=<your_key_secret>
export cmsRegion=cn-hangzhou
export cmsGroupId=1
```

### 5. 使用云监控进行失败告警

* copy操作

```bash
hadoop jar jindo-distcp-3.4.0.jar \
--src /data/incoming/hourly_table \
--dest oss://yang-hhht/hourly_table \
--enableCMS
```

* diff操作

```bash
hadoop jar jindo-distcp-3.4.0.jar \
--src /data/incoming/hourly_table \
--dest oss://yang-hhht/hourly_table \
--enableCMS
```