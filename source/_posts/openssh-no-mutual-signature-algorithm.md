---
title: 关于 `SSH send_pubkey_test: no mutual signature algorithm` 报错
---

## 背景
在 Windows 上 Git Bash 连接不上 gitlab 。同样的 SSH Key 在 macOS 上是可以用的。使用 `ssh -v git@github.org` 会报错：

```
debug1: Next authentication method: publickey
debug1: Offering public key: /c/Users/Admin/.ssh/id_rsa RSA SHA256:G9rBUQPNfglSIhDr8mfQO7QFDxk4ASQqQo2KNRg/0mo
debug1: send_pubkey_test: no mutual signature algorithm
```

报错显示没有共同的签名算法。

## 排查

使用 `ssh -vT git@github.com` 命令可以 Debug ：

```
-v Verbose mode. 
-T Disable pseudo-terminal allocation.
```

macOS 上的版本：

```
$ ssh -V
OpenSSH_8.6p1, LibreSSL 3.3.6
```

Win 上的版本：

```
$ ssh -V
OpenSSH_9.0p1, LibreSSL 3.3.6
```

## 原因与方案
根据 [OpenSSH to deprecate SHA-1 logins due to security risk](https://www.zdnet.com/article/openssh-to-deprecate-sha-1-logins-due-to-security-risk/)，是 **SHA-1** 算法不安全，OpenSSH （8.8开始） 不再支持 **ssh-rsa** 公钥签名算法（public key signature algorithm）了。需要改用 `ssh-ed25519` 算法：

```
$ ssh-keygen -t ed25519 -C "注释" -b 4096
```

## 参考文档
1. [ssh-rsa验证失败"no mutual signature algorithm"](https://zhuanlan.zhihu.com/p/419745598)
2. [RSA keys are not deprecated; SHA-1 signature scheme is!](https://ikarus.sg/rsa-is-not-dead/)