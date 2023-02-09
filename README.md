## 注意事项

如果本地配置了 .npmrc 为私有，记得用 `yarn install --registry https://registry.npmjs.org` 避免 Github workflow 失败。

## 开发

```bash
hexo s
```

## 部署：在 GitHub Pages 上部署 Hexo

参考官网 [在 GitHub Pages 上部署 Hexo](https://hexo.io/zh-cn/docs/github-pages) 设置 Github Workflows 实现自动部署。不要使用 `hexo deploy` 命令。当部署作业完成后，产生的页面会放在储存库中的 `gh-pages` 分支。