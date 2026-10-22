# Shadowrocket 共享分流规则

仅供 **Shadowrocket** 使用。规则来自 2026-10-22 本机当前启用的分流配置，共 234 条有效规则，保持原匹配顺序。

仓库只包含分流规则，不包含服务器节点、订阅地址、连接凭据、代理分组定义、DNS 或 HTTPS 解密配置。按照所有者的选择，规则中的自用服务器域名和 IP 保留并公开。

## 在每台设备上引用

1. 先确认 Shadowrocket 中已导入自己的节点，并已配置本机要使用的代理分组。
2. 进入 **配置 → 模块 → 右上角 ＋**，粘贴下方固定直链并下载。
3. 点击模块 → **编辑参数**，将各参数填为本机已有的代理分组名称。默认值对应源设备；如果只用一个代理，可将相应参数设为 `PROXY`。不需要复制分组定义或节点。
4. 确认这些策略均存在后启用模块。首页 **全局路由** 选择 **配置**。

```text
https://raw.githubusercontent.com/louisliu655-droid/shadowrocket-rules/main/shared-rules.sgmodule
```

这是远程规则模块，应添加到“模块”，不是首页的节点订阅。模块只有 `[Rule]`，含最终匹配 `FINAL`；启用后这些共享规则优先于本地配置的规则。若其他模块中也有分流规则，需按希望的优先级安排模块顺序。

| 模块参数 | 源设备默认分组 |
| --- | --- |
| ChatGPT | `chatgpt` |
| Gemini | `Gemini` |
| Claude | `CLAUDE` |
| AI | `ai` |
| TikTok | `Tiktok` |
| Video | `Watchthevideo` |
| SelfHosted | `自建的` |
| Auto | `所有中最快的` |
| Final | `其他都走这个` |

`DIRECT` 和 `PROXY` 为 Shadowrocket 内置策略，不需要建立同名分组。各设备的参数可各自保存，不随远程模块更新覆盖。

## 修改与更新

只维护 [`shared-rules.sgmodule`](shared-rules.sgmodule) 这一份文件。修改并提交到 `main` 后，各设备更新该远程模块；可在 Shadowrocket **设置 → 自动更新 → 模块** 中开启自动后台更新，并允许系统的后台 App 刷新。自动后台更新受系统调度影响，不代表提交后立即同步；需要立即生效时手动更新模块。

规则按文件从上到下匹配，`FINAL` 放在最后。策略参数采用 `{{{ChatGPT}}}` 等写法；请保持参数名称与文件头部声明一致。禁用模块即可恢复到原有本地配置和其他模块的规则。

## 导出处理说明

- 原数据库有 235 条非注释规则，另有 27 条注释。未修改原数据库或当前运行配置。
- 一条字段错位的旧规则没有导出为有效规则：原记录把 `domain-suffix` 当成域名、`EUDIC.NET` 当成策略；有效的 `eudic.net` 直连规则仍保留。
- 一条 `HOST,zxfw.court.gov.cn,DIRECT` 采用文档中的标准 `DOMAIN` 写法。
- 自定义策略通过模块参数映射到原分组名称，保持服务分流；不把它们全部合并为 `PROXY`。
- 保留重复规则及原有优先级，包括较早的 `meta.ai` → `ai` 优先于较晚的 ChatGPT 策略规则。
- 已检查规则结构、顺序、数量和凭据排除；尚未在其他设备实际导入验收。

参考：[Shadowrocket 使用手册：模块](https://github.com/LOWERTOP/Shadowrocket/wiki#模块)、[编辑参数](https://github.com/LOWERTOP/Shadowrocket/wiki#编辑参数)、[自动更新](https://github.com/LOWERTOP/Shadowrocket/wiki#自动更新)。
