# clash-rules

个人 Clash Verge 远程规则集（rule-provider 格式，非订阅）。

## 规则文件

- `my-rules.yaml` — 自定义规则集

## 在 Clash Verge 中使用

1. 订阅页面 → 新建 → 类型选 **Merge（覆写/合并）**。
2. 编辑该 Merge 配置，填入：

```yaml
rule-providers:
  my-github-rules:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/CXHW121/clash-rules/main/my-rules.yaml"
    path: ./ruleset/my-github-rules.yaml
    interval: 86400

prepend-rules:
  - RULE-SET,my-github-rules,节点选择
```

3. 启用该 Merge 配置，重启内核（Restart Core）后生效。

> 注意：`prepend-rules` 中第三个参数（策略组名称，如 `节点选择`）必须与当前订阅中真实存在的策略组名称一字不差，否则内核会报错无法启动。若拉取规则失败，可在 `url` 前加加速镜像（如 `https://ghproxy.net/`）。
