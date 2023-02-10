## 注意事项

如果本地配置了 .npmrc 为私有，记得用 `yarn install --registry https://registry.npmjs.org` 避免 Github workflow 失败。

## 开发

```bash
$ hexo s
```

### 使用 theme

```bash
$ git subtree add --prefix=themes/hexo-theme-next git@github.com:next-theme/hexo-theme-next.git master --squash
```

## 部署：在 GitHub Pages 上部署 Hexo

参考官网 [在 GitHub Pages 上部署 Hexo](https://hexo.io/zh-cn/docs/github-pages) 设置 Github Workflows 实现自动部署。不要使用 `hexo deploy` 命令。当部署作业完成后，产生的页面会放在储存库中的 `gh-pages` 分支。

## 站点 PV 统计
[展示 PV 与 UV 统计](https://fluid-dev.github.io/hexo-fluid-docs/guide/#%E7%BD%91%E9%A1%B5%E7%BB%9F%E8%AE%A1)