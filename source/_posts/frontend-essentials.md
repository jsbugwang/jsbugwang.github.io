---
title: 一些重点 JS 知识点的大杂烩
---

## npm semver
1. Caret Ranges^: 最左的非0锁定 - 默认，很多时候是锁 MAJOR
2. Tilde Ranges~: 指定了 MAJOR 或 MINOR 就锁定，仅不锁定 PATCH

## HTTP cache headers priority
### 优先级
```
Cache-Control > Expires > ETag > Last-Modified
```
### 浏览器行为
1. F5忽略强缓存头 （Cache-Control、Expires）
2. CTRL + F5 忽略所有4个缓存头

### 问题：如果做了强缓存，还需要弱缓存吗？
最好需要。这样用户按 F5 刷新时，弱缓存依然起作用。


