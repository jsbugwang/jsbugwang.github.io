## 一个没啥用的奇怪的  Git 技能 —— 给你的 Commit 加上 Verified 已认证

## 问题

先来看一张图，这是怎么回事呢？

![](https://lf3-static.bytednsdoc.com/obj/eden-cn/lm-pa/ljhwZthlaukjlkulzlp/static/img/git-verified-fake.jpeg)

这图不是 P 的，不信点击 [Github](https://github.com/jsbugwang/cloudflare-workers-short-url) 进去看看。原理如下代码和注释说明：

```bash
$ git clone git@github.com:jsbugwang/cloudflare-workers-short-url.git && cd cloudflare-workers-short-url
$ git config user.name "Linus Torvalds" # 名称在 git log 起作用，在 Github/Gitlab 如果能用  email 关联上账号则不起作用
$ git config user.email torvalds@linux-foundation.org # 核心是这个可以随便填写
$ echo -e "\nTest" >> README.md
$ git add README.md
$ git comit -m "真·Linus本人提交"
$ git push 
```

## 如何解决呢

答案是 Github/Gitlab 都支持给提交签名认证的功能。官网非常完善：[Sign commits with GPG](https://docs.gitlab.com/ee/user/project/repository/gpg_signed_commits/) 。下面是我的操作步骤，和官网一样的，我的多了一些有点用的 ⚠️ 注意事项和最后效果对比图。

### 操作步骤

#### 1. Create a GPG key （生成 GPG key）

```bash
$ gpg --full-gen-key
```

输入姓名（Real name）、email，操作系统弹框提示输入 passphrase ，完成。

> ⚠️：**passphrase** 务必记住，首次 commit 时会弹框输入。

> ⚠️：email 必须和 Github/Gitlab 账号一致。 否则显示 `Unverified: No user is associated with the committer email.` 如图：
> ![](https://lf3-static.bytednsdoc.com/obj/eden-cn/lm-pa/ljhwZthlaukjlkulzlp/static/img/No-user-is associated.png)

#### 2. Add a GPG key to your account （给 Github/Gitlab 账号添加 GPG key）

获取公钥：

```bash
$ gpg --list-secret-keys --keyid-format LONG <EMAIL> # 查看到第一行 sec  rsa3072/ 后面的 16 位就是 key ID
$ gpg --armor --export <ID>
```

把得到的公钥添加到 `settings → GPG Keys` 

> ⚠️：注意，添加到 Github/Gitlab 后检查下，应该显示绿色的 Verified ，说明 GPG key 的 email 和 Github/Gitlab 账号匹配上了！

#### 3. Associate your GPG key with Git （给本地 Git 关联 GPG key）

设置：

```
$ git config user.signingkey <ID>
```
> ⚠️：注意，如果你只有一个 Git 账号，可以 cofig --global 。我因为有多个，所以在项目中设置。

#### 4. Sign your Git commits （用 GPG key 提交代码）

有 2 种方式：

1. `git commit -S -m "My commit message"` 单次提交
2. `git config --global commit.gpgsign true` 每次提交

最后验收如图：
![](https://lf3-static.bytednsdoc.com/obj/eden-cn/lm-pa/ljhwZthlaukjlkulzlp/static/img/verified.png)

冒充不了啦，点击[这里看](https://github.com/jsbugwang/cloudflare-workers-short-url/commits/main)。

![](https://lf3-static.bytednsdoc.com/obj/eden-cn/lm-pa/ljhwZthlaukjlkulzlp/static/img/compare.jpg)

## Further Reading

什么是 GPG 呢？参考 [GPG入门教程 - 阮一峰](https://www.ruanyifeng.com/blog/2013/07/gpg.html)

> 1991年，程序员Phil Zimmermann为了避开政府监视，开发了加密软件PGP。这个软件非常好用，迅速流传开来，成了许多程序员的必备工具。但是，它是商业软件，不能自由使用。所以，自由软件基金会决定，开发一个PGP的替代品，取名为GnuPG。这就是GPG的由来。

简单说就是**加密软件**。
