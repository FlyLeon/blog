---
title: "Gpg 使用教程"
date: 2021-09-29T15:13:42+08:00
categories : ["archlinux"]
tags : ["gpg"]
---
> GGP（英语：Pretty Good Privacy，直译：优良保密协议）是一套用于讯息加密、验证的应用程式。

PGP的主要开发者是菲尔·齐默曼。齐默曼于1991年将PGP在互联网上免费发布。PGP本身是商业应用程序；开源并具有同类功能的工具名为GnuPG（GPG）。PGP及其同类产品均遵守OpenPGP数据加解密标准（RFC 4880）。    -维基百科

- 生成密码对

生成ECC
```
gpg --expert --full-gen-key
# 输出

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
   (9) ECC and ECC
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (13) Existing key
  (14) Existing key from card
```
选择9，继续
```
Please select which elliptic curve you want
   (1) Curve 25519
   (3) NIST P-256
   (4) NIST P-384
   (5) NIST P-521
   (6) Brainpool P-256
   (7) Brainpool P-384
   (8) Brainpool P-512
   (9) secp256k1            # 比特币使用的算法
```
选择1。
- 查看
```
gpg --list-secret-keys --keyid-format LONG
```
- 备份
```
gpg -o private.gpg --export-options backup --export-secret-keys emails

```
- 恢复
```
gpg --import-options restore --import private.gpg
```
