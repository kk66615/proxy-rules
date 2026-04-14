# Anthropic / Claude Rule Set

防止访问 Claude / Anthropic 时,主域名走代理而风控接口走直连,
导致 IP 来源不一致被判定异常并触发风控。

## 文件说明

| 文件 | 格式 | 适用 |
|---|---|---|
| `anthropic.yaml` | Clash classical provider | Mihomo / Stash / ClashX Pro |
| `anthropic.list` | 通用文本规则集 | Surge / Loon / QX / Mihomo |

## Mihomo (Clash Meta) 用法

```yaml
rule-providers:
  anthropic:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://raw.githubusercontent.com/kk66615/proxy-rules/main/rules/anthropic.yaml
    path: ./ruleset/anthropic.yaml

rules:
  - RULE-SET,anthropic,AI 专用
```

## Surge 用法

```ini
[Rule]
RULE-SET,https://raw.githubusercontent.com/kk66615/proxy-rules/main/rules/anthropic.list,AI 专用

[Proxy Group]
AI 专用 = select, 香港节点, 美国节点, ...
```

## Quantumult X 用法

在 `[filter_remote]` 段添加:

```
https://raw.githubusercontent.com/kk66615/proxy-rules/main/rules/anthropic.list, tag=Anthropic, force-policy=AI 专用, update-interval=86400, opt-parser=true, enabled=true
```

## 注意事项

1. **必须把所有规则全部走同一出口** (同一国家/同一 VPS 商更佳)。
2. `IP-ASN` 规则仅 Mihomo / Surge 支持,经典 Clash 请删除该行。
3. 监控/风控相关域名 (Sentry / Datadog / Sift / Statsig) 一旦放过直连,
   会立刻触发账号风险检测,**勿改**。
4. Stripe 支付域名**必须**与主站同出口,否则订阅/付款时 IP 不一致会触发风控。
5. `sentry.io`、`stripe.com` 等域名被广泛使用,
   本规则会将所有网站对这些服务的请求也路由至代理,属正常取舍。
6. 规则更新周期建议 1 天,Anthropic 偶尔会调整 CDN 后端。
