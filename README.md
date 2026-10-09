# clash-rules

个人 Clash Verge 远程规则集（rule-provider 格式，非订阅），按用途拆分：

- `direct.yaml` — 直连规则集（命中的流量走 DIRECT）
- `proxy.yaml` — 代理规则集（命中的流量走代理）

## 在 Clash Verge（Rev 2.x）中使用

> 注意：**不要**使用 `prepend-rules` 写进 Merge 文件——Clash Verge Rev 2.x 已不支持该语法（内核也不识别），规则不会生效。

### 1. Merge 文件（定义两个规则集来源）

```yaml
rule-providers:
  my-direct-rules:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/CXHW121/clash-rules/refs/heads/main/direct.yaml"
    path: ./ruleset/my-direct-rules.yaml
    interval: 86400
  my-proxy-rules:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/CXHW121/clash-rules/refs/heads/main/proxy.yaml"
    path: ./ruleset/my-proxy-rules.yaml
    interval: 86400
```

### 2. Rules 增强文件（前插到规则链最前）

```yaml
prepend:
  - RULE-SET,my-direct-rules,DIRECT
  - RULE-SET,my-proxy-rules,🚀 节点选择

append: []

delete: []
```

> 说明：
> - 出口策略由引用行决定，规则集文件只描述"匹配什么"（拆分的意义：同一条规则集合可由不同引用方决定走向）
> - `🚀 节点选择` 为策略组名，必须与当前订阅中真实存在的名称一字不差（含 emoji），否则内核启动失败
> - 两条引用的先后顺序即匹配优先级：先直连集、后代理集
> - 修改后需在 Clash Verge 重新应用配置并重启内核后生效
> - 若拉取失败（国内直连 GitHub 不稳定），可在 `url` 前加加速镜像（如 `https://ghproxy.net/`）
