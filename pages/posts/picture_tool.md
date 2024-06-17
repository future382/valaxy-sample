---
title: 简析图片无损放大工具	
date: 2023-7-08
excerpt: 站长必备
tags: 
    - 利器
    - 图片压缩算法
categories: 云迹的小工具
---



**图片模糊不清晰**的问题相信大家都遇到过。由于压缩、截图等原因，表情包、图片传来传去，越来越糊，慢慢变得和打了“马赛克”一样。
有时我们只有分辨率低、尺寸小、模糊不清的 N 手图片，又找不到清晰的高分辨率原图怎么办呢？这时候就需要「**图片无损放大利工具**」出场了

### 1. 佐糖

- 地址： https://picwish.cn/image-enlarger

官网介绍：使用人工智能深度学习技术，将放大后的图片噪点和锯齿部分进行补充，实现画质高清。针对放大后图片的线条、颜色、网点等，采用特殊算法处理，增强图片细节。其中不光有图片无损放大功能，还有图片去水印等等。并且有 Windows，macOS，iOS，Android 端。

#### 并且所有服务完全免费

### 2. Bigjpg – AI 人工智能图片无损放大

- 地址： https://bigjpg.com/
- 说明：文件大小 ≤ 10MB，尺寸 ≤ 3000x3000px

免费版可将图片放大 2~4 倍，可选择不同程度的图片降噪，升级后最大放大 16 倍。支持 Windows、Mac 和 安卓客户端使用。



### 3. **waifu2x**

- 地址： https://waifu2x.udp.jp/
- 说明：文件大小 ≤ 5MB，可降噪图像 ≤ 3000x3000px，可放大图像 ≤ 1500x1500px

来自 GitHub 上的一个著名的开源项目，使用深度卷积神经网络实现图片无损放大。可选择插图或照片，放大 1~2 倍，支持降噪处理。
还可以使用第三方开源 Windows 客户端 waifu2x-caffe，支持批量处理，更大的放大倍数和更多的优化选项。

- Github： https://github.com/lltcggie/waifu2x-caffe
- 下载：https://url66.ctfile.com/d/30717266-53601946-18d412?p=YPPW (访问密码: YPPW)

### 4. **AI Image Enlarger**

- 地址： https://imglarger.com/
- 说明：图片 ≤ 3MB，尺寸 ≤ 800×750，登录后 ≤ 2000×2000

放大 2~4 倍，自动分析图片噪点与锯齿，并恢复细节。

### 5. Squirrel Anime Enhance（win）

- Github：https://github.com/Justin62628/Squirrel-RIFE
- 下载：https://url66.ctfile.com/d/30717266-53601949-fc20a2?p=YPPW (访问密码: YPPW)

使用简单，并且支持视频，可以对视频进行补帧。随着 AI 的到来，对图片无损放大效果也越来越好了。 目前 AI 无损放大图片的项目有：waifu2x、Real-ESRGAN(腾讯出品)、以及今天要介绍的 Real-CUGAN (啊 B 出品)，主要针对动漫/动画图片进行算法，效果非常不错。 这三种 AI 算法效果可以通过下图看到 B 站出品的 Real-CUGAN 对纹理细节保留非常不错

waifu2x： models cunet：一般用于动漫超分 models photo：一般用于实拍 models style anime：一般用于老动漫
realCUGAN： 降噪版（Denoise）：主要针对原片噪声多； 无降噪版（No-Denoise）：如果原片噪声不多，压得还行，但是想提高分辨率/清晰度/做通用性的增强、修复处理，推荐使用； 保守版（conservative）：如果担心丢失纹理，担心画风被改变，颜色被增强，总之就是各种担心 AI 会留下浓重的处理痕迹，推荐使用该版本。



### 6. AI-Lossless-Zoomer (win)

- Github：https://github.com/X-Lucifer/AI-Lossless-Zoomer
- 下载:：https://url66.ctfile.com/d/30717266-53604739-55382c?p=YPPW (访问密码: YPPW)

AI-Lossless-Zoomer 是一款在 GitHub 开源的 Windows 图片无损放大软件。它采用了腾讯 ARC Lab 提供的图像超分辨率模型（Real-ESRGAN），能够很好地提高图片分辨率。
无论是真实照片、还是动漫图片、插画、表情包等，都有很好的放大效果。

[![68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f582d4c7563696665722f41492d4c6f73736c6573732d5a6f6f6d6572406d61737465722f737465702f312e706e67.webp](data:image/svg+xml;base64,PCEtLUFyZ29uTG9hZGluZy0tPgo8c3ZnIHdpZHRoPSIxIiBoZWlnaHQ9IjEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgc3Ryb2tlPSIjZmZmZmZmMDAiPjxnPjwvZz4KPC9zdmc+)](https://cdn.nlark.com/yuque/0/2023/webp/22578074/1672552450172-b779300c-5909-4506-a50c-ed1c06da95c2.webp#averageHue=%232c3039&clientId=ud28949a7-da57-4&from=ui&id=uf5957df3&name=68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f582d4c7563696665722f41492d4c6f73736c6573732d5a6f6f6d6572406d61737465722f737465702f312e706e67.webp&originHeight=682&originWidth=948&originalType=binary&ratio=1&rotation=0&showTitle=false&size=99486&status=done&style=none&taskId=uc59936da-1b80-471f-a782-1ae405ea524&title=)

- 功能说明：
  - 支持多线程处理
  - 支持批量图片处理
  - 支持设置选项
  - 支持自定义输出格式和自定义输出路径
  - 支持 AI 引擎选择
  - 支持批量清理任务