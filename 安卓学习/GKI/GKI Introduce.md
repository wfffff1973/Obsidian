google从android R开始推行的GKI(Generic Kernel Image)计划主要有以下考量：

1.  各SOC/OEM厂商kernel版本严重碎片化，kernel安全补丁以及其他补丁更新google无法掌控，对android生态带来不良影响；
2.  android版本碎片化较严重，SOC/OEM厂商为legacy MP project升级到最新的android 版本需要耗费巨大的资源和工作量。goolge为了降低android升级的工作量，GKI也是其中重要的一个环节。google定义KMI接口把GKI以外的其他module（包括kernel原生module和vendor自定义module）以ko档形式全部分离到GKI以外，模块和内核可以独立更新，真正做到了kernel core和driver module的去耦合化，大幅降低了更新kernel core和driver的efforct，同时也会大幅降低未来SOC/OEM升级kernel大版本时的工作量。
![](Pasted%20image%2020221227170547.png) 
![](Pasted%20image%2020221227170609.png)

同时google还引入了GRF Vendor Freeze机制，进一步降低厂商升级到新版android的工作量，对已经支持GKI的SOC platform，主要的工作量就是保持定期更新ACK patch以及执行相关测试工作。

MTK 2021 年new launched SOC平台DX-1/DX-P在android S开始支持GKI 2.0(based on android common kernel android12-5.10)，对已经MP的legacy platform，MTK仍然保持S版本主基线分支每个月升级到ACK最新的release版本，并且会持续基于最新的ACK release版本执行兼容性、功能、稳定性、性能、功耗等测试，确保ACK升级后的SW Quality符合预期。

对于定期拿主基线分支(s0.mp1/t0.mp1)的SW Git Release的客户来说，主要的工作量就是升级SW大版本，不需要从google直接获取ACK monthly release去merge。每个SW release的GKI版本资讯可以参考对应的release note -> [Label Info] sheet. release note路径：alps/vendor/mediatek/release_note/<PLATFORM>/ReleaseNote_for_MT6xxx_alps-mp-xx.mpx.xlsx。另外也可以参考本Quick Start的GKI info for Common SW章节。

而拉SP(special) branch的客户，目前MTK设定的rule是不会在SP branch上定期合入ACK monthly release，需要客户根据自己的需求直接从google拿ACK monthly Release后自行做ack patch merge，具体可以参考下图google提供的模型。

![](Pasted%20image%2020221227170629.png)

由于导入GKI 2.0后，kernel module开发、调试、ACK版本升级等均发生了较大的变化，本quick start针对客户可能遇到的问题做了分类说明：

1.  kernel编译和kernel module开发相关
2.  ack patch merge流程说明
3.  每月MTK在主基线上执行完ack patch merge操作后的announcement，并对mege过程中遇到的问题和额外需要补打的patch做具体说明
4.  常见问题的FAQ

Android GKI相关的官方网站、页面：

-   GKI Announcements

[https://docs.partner.android.com/partners/announcements/gki/gki2022](https://docs.partner.android.com/partners/announcements/gki/gki2022)  

-   GKI introduction

[https://source.android.com/devices/architecture/kernel/generic-kernel-image](https://source.android.com/devices/architecture/kernel/generic-kernel-image)  
[https://source.android.com/devices/architecture/kernel/gki-versioning](https://source.android.com/devices/architecture/kernel/gki-versioning)  
[https://source.android.com/devices/architecture/kernel/abi-monitor](https://source.android.com/devices/architecture/kernel/abi-monitor)

-   GKI Development

[https://source.android.com/docs/core/architecture/kernel/gki-dev](https://source.android.com/docs/core/architecture/kernel/gki-dev)

-   GKI Release

[https://source.android.com/docs/core/architecture/kernel/gki-releases](https://source.android.com/docs/core/architecture/kernel/gki-releases)  
[https://source.android.com/docs/core/architecture/kernel/gki-release-builds](https://source.android.com/docs/core/architecture/kernel/gki-release-builds)  
[https://android.googlesource.com/kernel/common/+refs](https://android.googlesource.com/kernel/common/+refs)