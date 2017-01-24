---
title: Let's Encrypt 证书申请
date: 2017-01-24 09:47:02
categories:
- 网站建设
tags:
- https
---
申请 *Let's Encrypt* 证书
<!-- more -->

**重要**：这篇笔记源于 [Jerry Qu](https://imququ.com) 的分享 《[Let's Encrypt，免费好用的 HTTPS 证书](https://imququ.com/post/letsencrypt-certificate.html)》

接下来的步骤均在一个预设的目录（比如 `SSL`）里执行:

# 一、创建私钥

```bash
# 创建私钥，用于识别身份
openssl genrsa 4096 > account.key
# 创建域名私钥
openssl genrsa 4096 > domain.key
```

# 二、 生成 CSR 文件

```bash
openssl req -new -sha256 -key domain.key -out domain.csr
```

# 三、配置验证服务

默认安装一下 `nginx` 就好了。

# 四、获取网站证书 CSR

```bash
# 下载 acme_tiny.py
wget https://raw.githubusercontent.com/diafygi/acme-tiny/master/acme_tiny.py
# 生成网站证书
python acme_tiny.py --account-key ./account.key --csr ./domain.csr --acme-dir /usr/share/nginx/html/.well-known/acme-challenge/ > signed.crt
```

# 五、下载中间证书

```bash
# 下载中间证书
wget -O - https://letsencrypt.org/certs/lets-encrypt-x3-cross-signed.pem > intermediate.pem
# 合并中间证书和网站证书
cat signed.crt intermediate.pem > chained.pem
```

# 六、配置 nginx 

```bash
ssl_certificate     ~/www/ssl/chained.pem;
ssl_certificate_key ~/www/ssl/domain.key;
```