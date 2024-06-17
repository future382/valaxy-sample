---
title: Let’s Encrypt | 通配符证书申请
date: 2023-4-30
tags: 
    - 利器
    - Web
    - SSL/TLS
categories: 云迹的小教程
excerpt: SSL证书申请
---



# Let’s Encrypt | 通配符证书申请

申请一个免费的SSL证书 还是挺容易的，途径很多，例如Let’s Encrypt这种机构，也不需要实名，只需证明你对具体的域名有‘拥有权’即可。

## 故事

这种‘拥有权’并不是法律意义上的拥有，说‘操作权’或许更合适，例如，即使这个域名法律上是属于某人A，被黑客B通过社会工程学把域名系统账号弄到手了，可以对该域名设置各种DNS记录，那么这个域名在Let’s Encrypt证书颁发机构看来，实际是属于黑客B的，仍然会为这个域名颁发证书，当然这个证书也就包含了黑客B所期望的信息。

相对于传统机构证书颁发方式，Let’s Encrypt这类机构不再核验 一大堆的法律上的，物理上的资料去校验这个域名的所属，而是仅仅通过实际的，谁可以操作这个域名，就颁发给谁，包含谁需要的信息。

这种方式其实是放宽了证书申请的门槛， 以前校验信息多多少少需要和现实主体扯上点联系，可能是个行政审批活，现在是个纯技术校验，完全虚拟世界内完成。

门槛降低，一定程度上安全性降低，可信度降低，所以大公司，政府等等行业还是会沿用以前的证书申请方式，同时Let’s Encrypt类似机构为了提升这种方式的安全性，证书的有效期只有天90天，过期重新申请。

这是一种思想解放，一种安全理念的改变，一种新的商业模式，Let’s Encrypt当时的横空出世时，搅动这个行业，现在看来，取得了成功。

## 如何验证’拥有权‘

![img](https://blog-samliu-tech-1300751433.cos.ap-shanghai.myqcloud.com/wp-content/uploads/2022/08/image-8.png)

- **子域名**

如上图，两步走。配合Certbot客户端程序，它会在Server端临时启动一个web服务，写入一些数据，然后告诉Let’s Encrypt，来访问ssl.yunji.blog，如果能成功访问到Server，则证明你对ssl.yunji.blog有“拥有权”，会颁发证书，你所需要做的就是配置ssl.yunji.blog指向Server的A记录并放开80/443端口即可。

- **二级域名**

如上图，三步走。例如yunji.blog这个二级域名，如果针对所有子域名一个个申请太繁琐，可以申请一个通配符证书，例如*.yunji.blog，这种证书申请的验证方式有些不同，只能使用DNS Challenge验证。

Certbot DNS Challenge ，首先在你的域名托管机构账户下，创建一个api token，要求具有读写DNS Zone的权限，然后添加token信息到Certbot，这样Certbot申请的时候，通过API，在你的DNS账户对应域名空间下创建一个TXT记录，最后Certbot告诉Let’s Encrypt，去读取这个TXT记录，如果一致，则证明你对yunji.blog有“拥有权”，会颁发通配符证书，颁发后，TXT记录，会被Certbot删除

![img](https://blog-samliu-tech-1300751433.cos.ap-shanghai.myqcloud.com/wp-content/uploads/2022/08/cloudflare_DNS-TXT-1024x50.jpg)

TXT记录

## Docker部署Certbot

部署前，先去弄一个二级域名，例如我在Namesilo购买的域名 yunji.blog,同时我的域名托管机构也是Namesilo，这里，我不在Namesilo账户申请api token，因为Certbot对Namesilo这个域名托管机构适配不太好，所以我把yunji.blog的托管机构从Namesilo迁移到了Cloudflare，迁移后，子域名的管理工作就交由Cloudflare，添加新记录时，同步到国内来，会稍微有些慢，不过![🆗](https://s.w.org/images/core/emoji/14.0.0/svg/1f197.svg)。

- **注册Cloudflare**
- **添加网站/域名**
- **更改Nmasilo **Nameservers**
- **等待一天，Cloudflare开始托管**
- **在Cloudflare，重新配置之前DNS记录**(**DNS ONLY**)
- **申请Cloudflare api token（只需编辑DNZ Zone 权限即可）**

[![img](https://blog-samliu-tech-1300751433.cos.ap-shanghai.myqcloud.com/wp-content/uploads/2022/08/image-9-1024x378.png)](https://blog-samliu-tech-1300751433.cos.ap-shanghai.myqcloud.com/wp-content/uploads/2022/08/image-9.png)

![img](https://blog-samliu-tech-1300751433.cos.ap-shanghai.myqcloud.com/wp-content/uploads/2022/08/image-10.png)

- **创建Certbot container**

```
docker run -d --name letsencrypt_testing \
--cap-add=NET_ADMIN \
-e PUID=1000 \
-e PGID=1000 \
-e TZ=Asia/Shanghai \
-e URL=yunji.blog \
-e SUBDOMAINS=wildcard \
-e VALIDATION=dns \
-e DNSPLUGIN=cloudflare \
-e DHLEVEL=2048 \
-v /home/yunji5342/letsencrypt-config_testing:/config \
--restart unless-stopped \
linuxserver/swag
```

- **添加Cloudfalre token**

```
sudo nano /home/yunji5342/letsencrypt-config_testing/dns-conf/cloudflare.ini
更改如下配置
 
# Instructions: https://github.com/certbot/certbot/blob/master/certbot-dns-cloudflare/certbot_dns_cloudflare/__init__.py#L20
# Replace with your values
 
# With global api key:
#dns_cloudflare_email = cloudflare@example.com
#dns_cloudflare_api_key = 0123456789abcdef0123456789abcdef01234567
 
# With token (comment out both lines above and uncomment below):
dns_cloudflare_api_token = xxxxxxxxxxxxxxxxxxx
```

- **重启container，完成申请过程，查看证书和私钥**

  

参考资料/工具：

#### ECC证书

https://imququ.com/post/ecc-certificate.html
https://blog.cloudflare.com/keyless-ssl-the-nitty-gritty-technical-details/
https://cloud.tencent.com/developer/article/1426836?from=article.detail.1420663
https://github.com/acmesh-official/acme.sh

#### 证书测试工具

[https://cipherlist.eu](https://cipherlist.eu/) tls chips配置最佳实践
