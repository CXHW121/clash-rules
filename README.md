# clash-rules

个人 Clash Verge 远程规则集（rule-provider 格式，非订阅）。

## 规则文件

- `my-rules.yaml` — 自定义规则集

## 在 Clash Verge（Rev 2.x）中使用

> 注意：**不要**使用 `prepend-rules` 写进 Merge 文件——Clash Verge Rev 2.x 已不支持该语法（内核也不识别），规则不会生效。正确做法分两处配置：

### 1. Merge 文件（定义规则集来源）

订阅页面 → Merge（覆写/合并）配置，填入：

```yaml
rule-providers:
  my-github-rules:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/CXHW121/clash-rules/refs/heads/main/my-rules.yaml"
    path: ./ruleset/my-github-rules.yaml
    interval: 86400
```

### 2. Rules 增强文件（把规则前插到规则链最前）

订阅卡片 → 编辑 → Rules（规则）增强，填入：

```yaml
prepend:
  - RULE-SET,my-github-rules,DIRECT

append: []

delete: []
```

> 说明：
> - `prepend` 将规则插入订阅规则列表最前面，避免被订阅自带的兜底 MATCH 规则拦截
> - `RULE-SET` 第三个参数是规则集内规则**未指定策略**时使用的默认策略；本规则集内规则自带 `DIRECT`，此处写 `DIRECT` 保持一致（写不存在的策略组名会导致内核校验失败）
> - 修改后需在 Clash Verge 重新应用配置（点击订阅卡片）并重启内核后生效
> - 若拉取规则失败（国内直连 GitHub 不稳定），可在 `url` 前加加速镜像（如 `https://ghproxy.net/`）
