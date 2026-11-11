# IEPL 与 Pixiv 定制

基于上游 `powerfullz/override-rules` v2.10.1，源码提交 `a6dfd9063ff36588f600891046169951020124ae`。

## 导入定制脚本

在支持本地脚本导入的 Mihomo 客户端中导入 `convert.min.js`，并将该覆写绑定到目标订阅。`convert.js` 是同一逻辑的未压缩版本。

需要远程订阅时，将源码提交到自己的 fork 并经构建部署后，使用该仓库的产物地址，例如：

```text
https://raw.githubusercontent.com/Cola-Ace/override-rules/refs/heads/dist/convert.min.js
```

该地址在本次定制发布到 `dist` 后生效。SubStore 可在链接末尾使用 `#grouptype=1&threshold=1` 传参。

## 香港／日本 IEPL

- 新增「香港 IEPL节点」和「日本 IEPL节点」，默认自动测速；`grouptype=0/1/2` 分别为手动选择、自动测速、负载均衡。
- 节点名同时包含上游支持的香港／日本标记与 `IEPL` 即可匹配；`HK IEPL 01`、`IEPL HK 01`、`日本 iepl 01`、`IEPL JP 01` 均受支持。
- 普通香港／日本组排除 IEPL 节点。JS 枚举、`regex=true` 与静态 YAML 使用同一套地区匹配和排除条件。
- 两类 IEPL 各自参与上游的地区阈值判定：默认每组至少 2 个节点；全部达标地区（含 IEPL）不足 2 个时沿用上游展平逻辑。需要单节点地区组可设置 `threshold=1`。
- 动态 JS 根据节点字段排除落地节点与 Tailscale 节点；落地节点以 `dialer-proxy: "前置代理"` 识别。
- 同时有台湾及香港候选时，哔哩哔哩组提供直连、台湾、普通香港及香港 IEPL 中实际生成的组。

静态 YAML 已加入每类 2 个 IEPL 假节点，用于生成对应过滤组；这些假节点会在输出前移除。实际使用静态 YAML 时，客户端按真实订阅的节点名筛选组成员。需要根据真实节点字段识别落地／Tailscale 时，使用动态 JS。

## Pixiv

「Pixiv」独立策略组接管 `pixiv.net`、`pximg.net` 及其子域名，规则位于通用静态资源和兜底规则之前。可在客户端为该组单独选择出口。

规则直接写入配置，无需下载额外的 Pixiv 规则集。图标使用原 fork 已有图片的固定提交地址。

## 构建

```powershell
pnpm install --lockfile=false --ignore-scripts
pnpm run typecheck
pnpm exec node scripts/build.mjs
pnpm run generate
```

修改入口：`src/catalog/countries.ts`（IEPL）、`src/catalog/service-groups.ts`（服务候选）、`src/builders/rules.ts`（Pixiv）、`scripts/yaml_generator/fake_proxies.json`（静态生成输入）。修改源码后重新构建 `convert.js`、`convert.min.js` 和 `yamls/`。
