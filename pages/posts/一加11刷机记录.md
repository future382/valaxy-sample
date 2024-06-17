---
abbrlink: ''
categories: []
cover: https://s2.loli.net/2023/08/23/R94PAlSivtrV2X7.jpg
date: '2023-08-23T12:03:41.414458+08:00'
excerpt: Oneplus 11 刷机记录
tags:
- 刷机
- 维护
title: 一加11刷机记录
updated: '2024-05-26T11:46:12.232+08:00'
---
## 缘由

众所周知，国内手机厂商的os极其垃圾，好用的不多（不是完全没有，今个的主角oxygen os就是一个），加上最近国家网信办又出台了个sb规定[（详情见这篇）](https://blog.adki.site/post/59fcf7cde9d0/)，增进了我刷机的动力

由于父母手机都是oneplus，我用的还是他们退下来的机子（一加11），自然刷的就是它。

刷机过程省略，详情自行google

## 刷了之后的bug：（已经有办法解决）

解决bug刷ocdt分区即可，然后oplusstanvbk可以用OOS的，理论上能正常OTA。方法见下面链接。顺序应该不关键，建议在第一次进fastboot就刷。
[https://forum.xda-developers.com/t/maybe-a-proper-way-to-fix-issues-after-converting-color-to-oxygen.4583321/](https://forum.xda-developers.com/t/maybe-a-proper-way-to-fix-issues-after-converting-color-to-oxygen.4583321/)

已知新问题：

* 不支持北斗导航（xda上自己说的，我用似乎是可以扫到北斗的）
* 亮度调节异常，有时候仅亮度指示有问题，有时候屏幕爆闪
* 颜色模式不可调
* 连电脑有记录不弹usb模式选择，状态栏没有改模式的选项，只能去开发者选项调（~~bug~~  特性）
* 某些情况下会崩溃重启（不是很清楚什么情况，接电话时候崩溃过一次，导致通话记录也没有记下号码）也有人说ColorOS也会偶尔崩溃

OTA会直接刷死（循环重启）如果OTA了需要手动刷回原始oplusstanvbk（尽量在第一次重启时候就进fastboot，我循环重启几次之后进9008了，但放了一天自己好了？？？）。用下面的刷机方法手动更新看起来是可行的。

如果root了，建议先备份基带和字库（presist和modem？刚入门，黑话不太懂）。没root的话可以用KernelSU配合fastboot boot启动一个临时root的环境备份

fastboot刷机：

如果用OFP包，下面的文件中如果找不到，找一下同名扩展名是bin或者mbn的。只用IMAGE文件夹下的，找不到的文件可以跳过，如果提示关键分区不要刷，my_carrier必定失败，没影响

oplusstanvbk 分区不要刷，只能用ColorOS的，刷了循环重启见上面ocdt分区说明

指令：

```shell

adb reboot bootloader
# (解锁等自行搜索)
fastboot --set-active=a
# 新机似乎没有b分区？不知道为什么，也可能是我没理解对fastboot flash boot boot.img
fastboot flash vendor_boot vendor_boot.img
fastboot flash vbmeta vbmeta.img
fastboot flash vbmeta_system vbmeta_system.img
fastboot flash vbmeta_vendor vbmeta_vendor.img
# 进入fastbootd，用户模式情况下才可以访问system分区
fastboot reboot fastbootfastboot flash abl abl.img
fastboot flash aop aop.img
fastboot flash aop_config aop_config.img
fastboot flash bluetooth bluetooth.img
fastboot flash cpucp cpucp.img
fastboot flash devcfg devcfg.img
fastboot flash dsp dsp.img
fastboot flash dtbo dtbo.img
fastboot flash engineering_cdt engineering_cdt.img
fastboot flash featenabler featenabler.img
fastboot flash hyp hyp.img
fastboot flash imagefv imagefv.img
fastboot flash init_boot init_boot.img
fastboot flash keymaster keymaster.img
#不知道要不要刷，没敢刷modem
rem modem.img
rem fastboot flash oplusstanvbk oplusstanvbk.img
fastboot flash oplus_sec oplus_sec.img
fastboot flash qupfw qupfw.img
fastboot flash recovery recovery.img
fastboot flash shrm shrm.img
fastboot flash splash splash.img
fastboot flash tz tz.img
fastboot flash uefi uefi.img
fastboot flash uefisecapp uefisecapp.img
# 不知道有没有危险，看启动流程好像在fastboot前面。我刷了也能开机，怂可以不刷
fastboot flash xbl xbl.img
fastboot flash xbl_config xbl_config.img
fastboot flash xbl_ramdump xbl_ramdump.imgfastboot flash system system.img
fastboot flash system_dlkm system_dlkm.img
fastboot flash system_ext system_ext.img
fastboot flash vendor vendor.img
fastboot flash vendor_dlkm vendor_dlkm.img
fastboot flash product product.img
fastboot flash odm odm.imgfastboot flash my_bigball my_bigball.img
fastboot flash my_carrier my_carrier.img
fastboot flash my_engineering my_engineering.img
fastboot flash my_heytap my_heytap.img
fastboot flash my_manifest my_manifest.img
fastboot flash my_product my_product.img
fastboot flash my_region my_region.img
fastboot flash my_stock my_stock.img
```

缺少一个my_preload，不刷的话app还是国内app。my_preload可以在ofp包里找到，预装app可以在系统内手动卸载。如果依旧介意的话可以刷

之后清空user分区，重启进系统，不知道不清空会有什么问题
已经是氧的可以不清空

```shell
fastboot -w
fastboot reboot
```

此外，我手头root后没法ota，不是很确定原因，

## 出现过以下几个情况：

重启变砖（似乎是不能在magisk修补完直接重启，要回到系统更新界面重启）

dmverity is disabled报错，拒绝更新，或者更新下载到99%后失败（怀疑是上面的操作之后搞乱了vbmeta）

<!--more-->
