# IEPL / Pixiv 定制覆写

基于 powerfullz/override-rules v2.10.1。源码：[feat/iepl-pixiv](https://github.com/Cola-Ace/override-rules/tree/feat/iepl-pixiv)，发布源提交 `5a5aef4a6fd1a36e22f9a54f7d3c02cbe5e71c8c`。

## 在 SubStore 中使用

1. 在「文件管理」中新建 **Mihomo 配置**。
2. 来源选择你的订阅或组合订阅。
3. 在「脚本操作」中选择远程脚本，填入：

```text
https://raw.githubusercontent.com/Cola-Ace/override-rules/refs/heads/dist/convert.min.js
```

4. 生成预览并保存，将文件的分享／输出链接导入 Mihomo 客户端。

需要单节点地区组时，脚本链接使用：

```text
https://raw.githubusercontent.com/Cola-Ace/override-rules/refs/heads/dist/convert.min.js#grouptype=1&threshold=1
```

默认启用 Fake-IP；需要 Redir-Host 时再附加 `&fakeip=false`。参数用 `#` 开始，多个参数用 `&` 分隔。

## 定制行为

- 新增「香港 IEPL节点」「日本 IEPL节点」，支持 `IEPL HK`、`HK IEPL`、`IEPL JP`、`日本 iepl` 等名称；普通香港／日本组排除 IEPL。
- 默认每组至少 2 个节点；可用 `threshold=1` 降低阈值。全部达标地区不足 2 个时，沿用上游的节点展平布局。
- 新增「Pixiv」策略组，接管 `pixiv.net`、`pximg.net` 及其子域名。
- `convert.js` 为未压缩脚本；`yamls/` 提供 768 种静态配置组合。需要根据真实节点字段识别落地／Tailscale 时使用动态 JS。

完整说明见[源码使用文档](https://github.com/Cola-Ace/override-rules/blob/feat/iepl-pixiv/CUSTOMISATIONS.md)。发布来源及时间见 [manifest.json](manifest.json)。

## 验证

发布前完成类型检查、lint、112 项测试，并检查全部 768 份静态 YAML。
