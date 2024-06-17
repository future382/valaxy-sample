---
cover: https://s2.loli.net/2023/08/20/xPTvR9KkBFJif6y.jpg
title: 实战：快速刷课程学习时间记录
date: 2023-7-31
categories: 云迹的小教程
tags: 
	- 利器
	- 摸鱼
excerpt: 水课经历估计谁都有，今天整个不一样的
---



## 思路

云课堂的本质是一个（套壳的）网页，可以用抓包软件分析记录学习进度请求，再通过修改这个请求并重发来修改学习进度。（此处使用钉钉进行演示）

## 提示

该方法仅 PC 端可用（其实手机也不是不行）。

## 一、安装 Fiddler

[点击这里](https://telerik-fiddler.s3.amazonaws.com/fiddler/FiddlerSetup.exe)下载安装。

[如果连接失败，这个也可以](https://drive.adkimsm.club/api/raw/?path=/工具/FiddlerSetup.exe)（我的文件服务器）

## 二、设置 Fiddler 捕获 HTTPS 流量

打开 Fiddler，点击顶栏 “Tools”，在弹出的菜单中点击 “Options…”。

[![img](https://s2.loli.net/2023/07/31/AhJ4tda3xlmCjUo.png)](https://s2.loli.net/2023/07/31/oR4IMqsiNaWJ23B.png)

在弹出的窗口中切换到顶部的 “HTTPS” 菜单，勾选 “Capture HTTPS CONNECTs” 和 “Decrypt HTTPS traffic” 复选框，然后点击 OK。

勾选后会弹出安装证书的窗口，确认即可。

[![img](https://s2.loli.net/2023/07/31/om1M72qDyx3iATb.png)](https://img2018.cnblogs.com/blog/1571380/202002/1571380-20200216162251978-1347937089.png)

如果出现下面这样的黄条，点击黄条即可。

[![img](https://s2.loli.net/2023/07/31/oR4IMqsiNaWJ23B.png)](https://img2018.cnblogs.com/blog/1571380/202002/1571380-20200216162252290-2089367078.png)

## 三、写入脚本

在 Fiddler 中按下 `Ctrl+R`。弹出一个代码编辑器窗口。

[![img](https://s2.loli.net/2023/07/31/NcvZBbKsOYjMTfn.png)](https://img2018.cnblogs.com/blog/1571380/202002/1571380-20200216162252608-1173918758.png)

在代码编辑器窗口按下 `Ctrl+F`，在弹出的窗口中输入 OnBeforeResponse 并按下回车。找到 OnBeforeResponse 函数（下图所示）。

[![img](https://s2.loli.net/2023/07/31/behFgO4iT26DaRZ.png)](https://img2018.cnblogs.com/blog/1571380/202002/1571380-20200216162252982-624873870.png)

把

```
static function OnBeforeResponse(oSession: Session) {
    if (m_Hide304s && oSession.responseCode == 304) {
        oSession["ui-hide"] = "true";
    }
}
```

替换成下面的代码，**然后按下 `Ctrl+S` 来保存**。

```
    public static RulesOption("视频增强插件")
    var m_H5VideoPlayerExtension: boolean = true;
    public static RulesOption("自动学习")
    var m_AutoLearn: boolean = true;
    static function OnBeforeResponse(oSession: Session) {
        if (m_Hide304s && oSession.responseCode == 304) {
            oSession["ui-hide"] = "true";
        }
        var sToInsert = "";
        if (m_H5VideoPlayerExtension) {
            sToInsert += "<script src=data:application/javascript;base64,KGZ1bmN0aW9uKCl7dmFyIGh0bWxfcGxheWVyX2VuaGFuY2U9e2ZvbnRTaXplOjIwLHBsYXllcjpmdW5jdGlvbigpe3JldHVybiBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJ2aWRlbyIpfSx0aXBzOmZ1bmN0aW9uKHN0cil7dmFyIHN0eWxlPWRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoIiNodG1sX3BsYXllcl9lbmhhbmNlX3RpcHMiKS5zdHlsZTtkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCIjaHRtbF9wbGF5ZXJfZW5oYW5jZV90aXBzIikuaW5uZXJUZXh0PXN0cjtmb3IodmFyIGk9MDtpPDM7aSsrKXtpZih0aGlzLm9uX29mZltpXSl7Y2xlYXJUaW1lb3V0KHRoaXMub25fb2ZmW2ldKX19c3R5bGUuZGlzcGxheT0iYmxvY2siO3RoaXMub25fb2ZmWzBdPXNldFRpbWVvdXQoZnVuY3Rpb24oKXtzdHlsZS5vcGFjaXR5PTF9LDUwKTt0aGlzLm9uX29mZlsxXT1zZXRUaW1lb3V0KGZ1bmN0aW9uKCl7c3R5bGUub3BhY2l0eT0wfSwyMDAwKTt0aGlzLm9uX29mZlsyXT1zZXRUaW1lb3V0KGZ1bmN0aW9uKCl7c3R5bGUuZGlzcGxheT0ibm9uZSJ9LDI4MDApfSxvbl9vZmY6bmV3IEFycmF5KDMpLHJvdGF0ZTowLGZwczozMCxmaWx0ZXI6e2tleTpuZXcgQXJyYXkoNSksc2V0dXA6ZnVuY3Rpb24oKXt2YXIgdmlldz0iYnJpZ2h0bmVzcyh7MH0pIGNvbnRyYXN0KHsxfSkgc2F0dXJhdGUoezJ9KSBodWUtcm90YXRlKHszfWRlZykgYmx1cih7NH1weCkiO2Zvcih2YXIgaT0wO2k8NTtpKyspe3ZpZXc9dmlldy5yZXBsYWNlKCJ7IitpKyJ9IixTdHJpbmcodGhpcy5rZXlbaV0pKTt0aGlzLmtleVtpXT1OdW1iZXIodGhpcy5rZXlbaV0pfWh0bWxfcGxheWVyX2VuaGFuY2UucGxheWVyKCkuc3R5bGUuV2Via2l0RmlsdGVyPXZpZXd9LHJlc2V0OmZ1bmN0aW9uKCl7dGhpcy5rZXlbMF09MTt0aGlzLmtleVsxXT0xO3RoaXMua2V5WzJdPTE7dGhpcy5rZXlbM109MDt0aGlzLmtleVs0XT0wO3RoaXMuc2V0dXAoKX19LHNldHRpcHM6ZnVuY3Rpb24oKXt2YXIgdGlwcz1kb2N1bWVudC5jcmVhdGVFbGVtZW50KCJkaXYiKTt0aGlzLnBsYXllcigpLnBhcmVudE5vZGUuYXBwZW5kQ2hpbGQodGlwcyk7dGlwcy5zZXRBdHRyaWJ1dGUoImlkIiwiaHRtbF9wbGF5ZXJfZW5oYW5jZV90aXBzIik7dGlwcy5zZXRBdHRyaWJ1dGUoInN0eWxlIiwicG9zaXRpb246IGFic29sdXRlO3otaW5kZXg6IDk5OTk5OTtwYWRkaW5nOiAxMHB4O2JhY2tncm91bmQ6IHJnYmEoMCwwLDAsMC44KTtjb2xvcjp3aGl0ZTt0b3A6IDUwJTtsZWZ0OiA1MCU7dHJhbnNmb3JtOiB0cmFuc2xhdGUoLTUwJSwtNTAlKTt0cmFuc2l0aW9uOiBhbGwgNTAwbXMgZWFzZTtvcGFjaXR5OiAwO2Rpc3BsYXk6IG5vbmU7IC13ZWJraXQtZm9udC1zbW9vdGhpbmc6IHN1YnBpeGVsLWFudGlhbGlhc2VkO2ZvbnQtZmFtaWx5OiAnbWljcm9zb2Z0IHlhaGVpJywgVmVyZGFuYSwgR2VuZXZhLCBzYW5zLXNlcmlmOy13ZWJraXQtdXNlci1zZWxlY3Q6IG5vbmU7Iik7aWYodGhpcy5mb250U2l6ZSE9PTApe3RpcHMuc3R5bGUuZm9udFNpemU9dGhpcy5mb250U2l6ZSsicHgifWlmKGxvY2F0aW9uLmhvc3RuYW1lPT09Ind3dy55b3V0dWJlLmNvbSIpe3RoaXMucGxheWVyKCkucGFyZW50Tm9kZS5zdHlsZS5oZWlnaHQ9IjEwMCUifX0sX2lzRm91Y3M6ZmFsc2UsaXNGb3VjczpmdW5jdGlvbigpe3RoaXMucGxheWVyKCkub25tb3VzZW92ZXI9ZnVuY3Rpb24oKXtodG1sX3BsYXllcl9lbmhhbmNlLl9pc0ZvdWNzPXRydWV9O3RoaXMucGxheWVyKCkub25tb3VzZW91dD1mdW5jdGlvbigpe2h0bWxfcGxheWVyX2VuaGFuY2UuX2lzRm91Y3M9ZmFsc2V9fSxidXR0b246ZnVuY3Rpb24oZXZlbnQpe3ZhciBfdGhpcz1odG1sX3BsYXllcl9lbmhhbmNlO2lmKGV2ZW50LmFsdEtleXx8ZXZlbnQuY3RybEtleXx8ZXZlbnQuc2hpZnRLZXkpe3JldHVybn1pZighX3RoaXMuX2lzRm91Y3Mpe3JldHVybn1ldmVudC5zdG9wUHJvcGFnYXRpb24oKTtldmVudC5wcmV2ZW50RGVmYXVsdCgpO2lmKGV2ZW50LmtleUNvZGU9PT0zOSl7X3RoaXMucGxheWVyKCkuY3VycmVudFRpbWUrPTM7X3RoaXMudGlwcygi5b+r6L+b77yaM+enkiIpfWlmKGV2ZW50LmtleUNvZGU9PT0zNyl7X3RoaXMucGxheWVyKCkuY3VycmVudFRpbWUtPTM7X3RoaXMudGlwcygi5ZCO6YCA77yaM+enkiIpfWlmKGV2ZW50LmtleUNvZGU9PT0zOCl7aWYoX3RoaXMucGxheWVyKCkudm9sdW1lPDEpe190aGlzLnBsYXllcigpLnZvbHVtZSs9MC4wMX1fdGhpcy5wbGF5ZXIoKS52b2x1bWU9X3RoaXMucGxheWVyKCkudm9sdW1lLnRvRml4ZWQoMik7X3RoaXMudGlwcygi6Z+z6YeP77yaIitwYXJzZUludChfdGhpcy5wbGF5ZXIoKS52b2x1bWUqMTAwKSsiJSIpfWlmKGV2ZW50LmtleUNvZGU9PT00MCl7aWYoX3RoaXMucGxheWVyKCkudm9sdW1lPjApe190aGlzLnBsYXllcigpLnZvbHVtZS09MC4wMX1fdGhpcy5wbGF5ZXIoKS52b2x1bWU9X3RoaXMucGxheWVyKCkudm9sdW1lLnRvRml4ZWQoMik7X3RoaXMudGlwcygi6Z+z6YeP77yaIitwYXJzZUludChfdGhpcy5wbGF5ZXIoKS52b2x1bWUqMTAwKSsiJSIpfWlmKGV2ZW50LmtleUNvZGU9PT0zMil7aWYoX3RoaXMucGxheWVyKCkucGF1c2VkKXtfdGhpcy5wbGF5ZXIoKS5wbGF5KCk7X3RoaXMudGlwcygi5pKt5pS+Iil9ZWxzZXtfdGhpcy5wbGF5ZXIoKS5wYXVzZSgpO190aGlzLnRpcHMoIuaaguWBnCIpfX1pZihldmVudC5rZXlDb2RlPT09ODgpe2lmKF90aGlzLnBsYXllcigpLnBsYXliYWNrUmF0ZT4wKXtfdGhpcy5wbGF5ZXIoKS5wbGF5YmFja1JhdGUtPTAuMTtfdGhpcy5wbGF5ZXIoKS5wbGF5YmFja1JhdGU9X3RoaXMucGxheWVyKCkucGxheWJhY2tSYXRlLnRvRml4ZWQoMSk7X3RoaXMudGlwcygi5pKt5pS+6YCf5bqm77yaIitfdGhpcy5wbGF5ZXIoKS5wbGF5YmFja1JhdGUrIuWAjSIpfX1pZihldmVudC5rZXlDb2RlPT09Njcpe2lmKF90aGlzLnBsYXllcigpLnBsYXliYWNrUmF0ZTwxNil7X3RoaXMucGxheWVyKCkucGxheWJhY2tSYXRlKz0wLjE7X3RoaXMucGxheWVyKCkucGxheWJhY2tSYXRlPV90aGlzLnBsYXllcigpLnBsYXliYWNrUmF0ZS50b0ZpeGVkKDEpO190aGlzLnRpcHMoIuaSreaUvumAn+W6pu+8miIrX3RoaXMucGxheWVyKCkucGxheWJhY2tSYXRlKyLlgI0iKX19aWYoZXZlbnQua2V5Q29kZT09PTkwKXtfdGhpcy5wbGF5ZXIoKS5wbGF5YmFja1JhdGU9MTtfdGhpcy50aXBzKCLmkq3mlL7pgJ/luqbvvJox5YCNIil9aWYoZXZlbnQua2V5Q29kZT09NzApe2lmKCFfdGhpcy5wbGF5ZXIoKS5wYXVzZWQpe190aGlzLnBsYXllcigpLnBhdXNlKCl9X3RoaXMucGxheWVyKCkuY3VycmVudFRpbWUrPU51bWJlcigxL190aGlzLmZwcyk7X3RoaXMudGlwcygi5a6a5L2N77ya5LiL5LiA5binIil9aWYoZXZlbnQua2V5Q29kZT09Njgpe2lmKCFfdGhpcy5wbGF5ZXIoKS5wYXVzZWQpe190aGlzLnBsYXllcigpLnBhdXNlKCl9X3RoaXMucGxheWVyKCkuY3VycmVudFRpbWUtPU51bWJlcigxL190aGlzLmZwcyk7X3RoaXMudGlwcygi5a6a5L2N77ya5LiK5LiA5binIil9aWYoZXZlbnQua2V5Q29kZT09Njkpe2lmKF90aGlzLmZpbHRlci5rZXlbMF0+MSl7X3RoaXMuZmlsdGVyLmtleVswXSs9MX1lbHNle190aGlzLmZpbHRlci5rZXlbMF0rPTAuMX1fdGhpcy5maWx0ZXIua2V5WzBdPV90aGlzLmZpbHRlci5rZXlbMF0udG9GaXhlZCgyKTtfdGhpcy5maWx0ZXIuc2V0dXAoKTtfdGhpcy50aXBzKCLlm77lg4/kuq7luqblop7liqDvvJoiK3BhcnNlSW50KF90aGlzLmZpbHRlci5rZXlbMF0qMTAwKSsiJSIpfWlmKGV2ZW50LmtleUNvZGU9PTg3KXtpZihfdGhpcy5maWx0ZXIua2V5WzBdPjApe2lmKF90aGlzLmZpbHRlci5rZXlbMF0+MSl7X3RoaXMuZmlsdGVyLmtleVswXS09MX1lbHNle190aGlzLmZpbHRlci5rZXlbMF0tPTAuMX1fdGhpcy5maWx0ZXIua2V5WzBdPV90aGlzLmZpbHRlci5rZXlbMF0udG9GaXhlZCgyKTtfdGhpcy5maWx0ZXIuc2V0dXAoKX1fdGhpcy50aXBzKCLlm77lg4/kuq7luqblh4/lsJHvvJoiK3BhcnNlSW50KF90aGlzLmZpbHRlci5rZXlbMF0qMTAwKSsiJSIpfWlmKGV2ZW50LmtleUNvZGU9PTg0KXtpZihfdGhpcy5maWx0ZXIua2V5WzFdPjEpe190aGlzLmZpbHRlci5rZXlbMV0rPTF9ZWxzZXtfdGhpcy5maWx0ZXIua2V5WzFdKz0wLjF9X3RoaXMuZmlsdGVyLmtleVsxXT1fdGhpcy5maWx0ZXIua2V5WzFdLnRvRml4ZWQoMik7X3RoaXMuZmlsdGVyLnNldHVwKCk7X3RoaXMudGlwcygi5Zu+5YOP5a+55q+U5bqm5aKe5Yqg77yaIitwYXJzZUludChfdGhpcy5maWx0ZXIua2V5WzFdKjEwMCkrIiUiKX1pZihldmVudC5rZXlDb2RlPT04Mil7aWYoX3RoaXMuZmlsdGVyLmtleVsxXT4wKXtpZihfdGhpcy5maWx0ZXIua2V5WzFdPjEpe190aGlzLmZpbHRlci5rZXlbMV0tPTF9ZWxzZXtfdGhpcy5maWx0ZXIua2V5WzFdLT0wLjF9X3RoaXMuZmlsdGVyLmtleVsxXT1fdGhpcy5maWx0ZXIua2V5WzFdLnRvRml4ZWQoMik7X3RoaXMuZmlsdGVyLnNldHVwKCl9X3RoaXMudGlwcygi5Zu+5YOP5a+55q+U5bqm5YeP5bCR77yaIitwYXJzZUludChfdGhpcy5maWx0ZXIua2V5WzFdKjEwMCkrIiUiKX1pZihldmVudC5rZXlDb2RlPT04NSl7aWYoX3RoaXMuZmlsdGVyLmtleVsyXT4xKXtfdGhpcy5maWx0ZXIua2V5WzJdKz0xfWVsc2V7X3RoaXMuZmlsdGVyLmtleVsyXSs9MC4xfV90aGlzLmZpbHRlci5rZXlbMl09X3RoaXMuZmlsdGVyLmtleVsyXS50b0ZpeGVkKDIpO190aGlzLmZpbHRlci5zZXR1cCgpO190aGlzLnRpcHMoIuWbvuWDj+mlseWSjOW6puWinuWKoO+8miIrcGFyc2VJbnQoX3RoaXMuZmlsdGVyLmtleVsyXSoxMDApKyIlIil9aWYoZXZlbnQua2V5Q29kZT09ODkpe2lmKF90aGlzLmZpbHRlci5rZXlbMl0+MCl7aWYoX3RoaXMuZmlsdGVyLmtleVsyXT4xKXtfdGhpcy5maWx0ZXIua2V5WzJdLT0xfWVsc2V7X3RoaXMuZmlsdGVyLmtleVsyXS09MC4xfV90aGlzLmZpbHRlci5rZXlbMl09X3RoaXMuZmlsdGVyLmtleVsyXS50b0ZpeGVkKDIpO190aGlzLmZpbHRlci5zZXR1cCgpfV90aGlzLnRpcHMoIuWbvuWDj+mlseWSjOW6puWHj+Wwke+8miIrcGFyc2VJbnQoX3RoaXMuZmlsdGVyLmtleVsyXSoxMDApKyIlIil9aWYoZXZlbnQua2V5Q29kZT09Nzkpe190aGlzLmZpbHRlci5rZXlbM10rPTE7X3RoaXMuZmlsdGVyLnNldHVwKCk7X3RoaXMudGlwcygi5Zu+5YOP6Imy55u45aKe5Yqg77yaIitfdGhpcy5maWx0ZXIua2V5WzNdKyLluqYiKX1pZihldmVudC5rZXlDb2RlPT03Myl7X3RoaXMuZmlsdGVyLmtleVszXS09MTtfdGhpcy5maWx0ZXIuc2V0dXAoKTtfdGhpcy50aXBzKCLlm77lg4/oibLnm7jlh4/lsJHvvJoiK190aGlzLmZpbHRlci5rZXlbM10rIuW6piIpfWlmKGV2ZW50LmtleUNvZGU9PTc1KXtfdGhpcy5maWx0ZXIua2V5WzRdKz0xO190aGlzLmZpbHRlci5zZXR1cCgpO190aGlzLnRpcHMoIuWbvuWDj+aooeeziuWinuWKoO+8miIrX3RoaXMuZmlsdGVyLmtleVs0XSsiUFgiKX1pZihldmVudC5rZXlDb2RlPT03NCl7aWYoX3RoaXMuZmlsdGVyLmtleVs0XT4wKXtfdGhpcy5maWx0ZXIua2V5WzRdLT0xO190aGlzLmZpbHRlci5zZXR1cCgpfV90aGlzLnRpcHMoIuWbvuWDj+aooeeziuWHj+Wwke+8miIrX3RoaXMuZmlsdGVyLmtleVs0XSsiUFgiKX1pZihldmVudC5rZXlDb2RlPT04MSl7X3RoaXMuZmlsdGVyLnJlc2V0KCk7X3RoaXMudGlwcygi5Zu+5YOP5bGe5oCn77ya5aSN5L2NIil9aWYoZXZlbnQua2V5Q29kZT09ODMpe190aGlzLnJvdGF0ZSs9OTA7aWYoX3RoaXMucm90YXRlJTM2MD09PTApe190aGlzLnJvdGF0ZT0wfV90aGlzLnBsYXllcigpLnN0eWxlLnRyYW5zZm9ybT0icm90YXRlKCIrX3RoaXMucm90YXRlKyJkZWcpIjtfdGhpcy50aXBzKCLnlLvpnaLml4vovazvvJoiK190aGlzLnJvdGF0ZSsi5bqmIil9aWYoZXZlbnQua2V5Q29kZT09MTMpe2lmKGxvY2F0aW9uLmhvc3RuYW1lPT09Ind3dy5iaWxpYmlsaS5jb20iKXtpZihkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCdbZGF0YS10ZXh0PSLov5vlhaXlhajlsY8iXScpKXtkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCdbZGF0YS10ZXh0PSLov5vlhaXlhajlsY8iXScpLmNsaWNrKCl9fWlmKGxvY2F0aW9uLmhvc3RuYW1lPT09Ind3dy55b3V0dWJlLmNvbSIpe2lmKGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoJ1tjbGFzcz0ieXRwLWZ1bGxzY3JlZW4tYnV0dG9uIHl0cC1idXR0b24iXScpKXtkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCdbY2xhc3M9Inl0cC1mdWxsc2NyZWVuLWJ1dHRvbiB5dHAtYnV0dG9uIl0nKS5jbGljaygpfX19fSxpbml0OmZ1bmN0aW9uKCl7aWYoZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgiI2h0bWxfcGxheWVyX2VuaGFuY2VfdGlwcyIpLmxlbmd0aD4xKXtkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCIjaHRtbF9wbGF5ZXJfZW5oYW5jZV90aXBzIikucGFyZW50Tm9kZS5yZW1vdmVDaGlsZChkb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCIjaHRtbF9wbGF5ZXJfZW5oYW5jZV90aXBzIilbMV0pfWlmKGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3JBbGwoInZpZGVvIikubGVuZ3RoPT09MSYmZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgiI2h0bWxfcGxheWVyX2VuaGFuY2VfdGlwcyIpLmxlbmd0aD09PTApe2lmKCF0aGlzLmxvYWQpe3ZhciBfdGhpcz1odG1sX3BsYXllcl9lbmhhbmNlO3RoaXMubG9hZD10cnVlO3NldFRpbWVvdXQoZnVuY3Rpb24oKXtjb25zb2xlLmxvZygi5qOA5rWL5YiwSFRNTDXop4bpopHvvIEiKTtjb25zb2xlLmxvZyghX3RoaXMubG9hZCk7X3RoaXMubG9hZD1mYWxzZTtfdGhpcy5maWx0ZXIucmVzZXQoKTtfdGhpcy5zZXR0aXBzKCk7X3RoaXMuaXNGb3VjcygpO2RvY3VtZW50Lm9ua2V5ZG93bj1fdGhpcy5idXR0b259LDEwMDApfX19LGxvYWQ6ZmFsc2V9O2h0bWxfcGxheWVyX2VuaGFuY2UuaW5pdCgpO2RvY3VtZW50LmFkZEV2ZW50TGlzdGVuZXIoIkRPTU5vZGVJbnNlcnRlZCIsZnVuY3Rpb24oKXtodG1sX3BsYXllcl9lbmhhbmNlLmluaXQoKX0pfSkoKTs=></script>";
        }
        oSession.utilDecodeResponse();
        oSession.utilReplaceOnceInResponse('</head>', sToInsert + '</head>', 0);
        if (m_AutoLearn && !oSession.GetRequestBodyAsString().Contains("\"courseTime\":9990,\"learnTime\":60,\"type\":2")
            && oSession.hostname == "saas.daxue.dingtalk.com" && oSession.PathAndQuery == "/dingtalk/course/record.jhtml"){
            FiddlerApplication.Log.LogString("FOUND!!!");
            var raw = "";
            var method:String = oSession.RequestMethod;
            var url:String = oSession.fullUrl;
            var protocol = "HTTP/1.1";
            FiddlerApplication.Log.LogString(method + " " + url + " " + protocol);
            raw += method + " " + url + " " + protocol + "\r\n";
            var body = oSession.GetRequestBodyAsString();
            body = body.replace(/"courseTime":\d+,"learnTime":\d+,"type":\d/g,"\"courseTime\":9990,\"learnTime\":60,\"type\":2");
            FiddlerApplication.Log.LogString(body.Length);
            for (var i:int = 0; i < oSession.oRequest.headers.Count(); i++) {
                var header = oSession.oRequest.headers[i];
                header = header.ToString()
                FiddlerApplication.Log.LogString(header)
                if(header.Contains("Content-Length")){
                    header = "Content-Length: "+(body.Length).ToString()
                }
                raw += header + "\r\n";
            }
            raw += "\r\n" + body;
            for (var j:int = 0; j < 10; j++) {
                FiddlerObject.utilIssueRequest(raw);
                FiddlerApplication.Log.LogString("Request has been Send.");
                System.Threading.Thread.Sleep(5000);
            }
        }
    }
```

## 四、看视频

看视频的时候，每次刷新页面（包括第一次进入视频页面时的加载）程序会花 1 min 时间给视频增加 10 min 的学习进度。保险起见，**请不要频繁刷新**。

Log 选项卡中，每出现一条 `Resquest has been Send.` 说明学习进度增加了 1 min。**快速出现大量 `Resquest has been Send.` 时，可能是刷新过于频繁或程序错误，请立刻关闭 Fiddler**。如果 Fiddler 未响应，可能是陷入死循环，立刻在任务管理器里结束进程。

[![img](https://s2.loli.net/2023/07/31/RZechAwYU71Q6yr.png)](https://img2018.cnblogs.com/blog/1571380/202002/1571380-20200219130953818-1951417504.png)

在 Rules 中可以启用或关闭 `视频增强插件` 和 `自动学习`。

[![img](https://s2.loli.net/2023/07/31/9lxdpyYsZDm2eQh.png)](https://img2018.cnblogs.com/blog/1571380/202002/1571380-20200219131043250-1212918436.png)