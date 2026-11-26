# proxy-pool

自动聚合多个公开免费节点源，去重后生成 **Clash** 和 **v2rayN** 两种订阅，每 6 小时由 GitHub Actions 自动更新。

## 订阅地址

| 客户端 | 订阅链接（墙内优先用镜像） |
|---|---|
| **Clash / Mihomo / FlClash / Clash Verge** | `https://gh-proxy.com/raw.githubusercontent.com/yongce-liu/proxy-pool/main/clash.yaml` |
| **v2rayN / v2rayNG / Shadowrocket** | `https://gh-proxy.com/raw.githubusercontent.com/yongce-liu/proxy-pool/main/v2ray.txt` |

**日常订阅优先使用上面的镜像链接**，尤其是 GitHub 原始地址访问不稳定时。Clash 用户只需要复制 Clash 那一行。

原始地址（海外或已有代理时可用）：

```
https://raw.githubusercontent.com/yongce-liu/proxy-pool/main/clash.yaml
https://raw.githubusercontent.com/yongce-liu/proxy-pool/main/v2ray.txt
```

仓库地址：<https://github.com/yongce-liu/proxy-pool>。GitHub Actions 每 6 小时自动更新订阅文件。

## Clash 版内置策略组

- **🚀 节点选择**：直接按国家/地区列出所有可用节点组，不再提供精选、全池自动或平铺全部节点入口
- **国家/地区组**：每个国家一个 url-test 组，只在该国节点内自动选择延迟最低的节点
- **未标注节点**：无法识别国家/地区的节点会在生成订阅时直接过滤，不发布到订阅文件
- 国别识别：优先解析节点名（国旗 emoji / 中文国名 / ISO 代码），再用 MaxMind 本地库补识别，最后用 `ip.sb` / `ipinfo.io` 查询 IP 归属地

策略组层级如下：

```
🚀 节点选择
├── 🇭🇰 香港·74
├── 🇯🇵 日本·65
├── 🇺🇸 美国·447
├── 🇸🇬 新加坡·107
├── ...
└── DIRECT
```

国家组名称会带节点数量（例如 `🇯🇵 日本·65`），方便判断该组规模。未标注节点不会进入任何组。

节点名称会统一缩短为“国旗 + 国家/地区 + 序号”，例如：

```text
🇯🇵 日本 01
🇺🇸 美国 01
🇺🇸 美国 02
```

不再保留原始来源前缀和长节点名，方便在客户端里查看。

分流规则：内网 IP 直连 → GEOIP CN 直连 → 其余走代理。

## 使用建议

1. 客户端把订阅**自动更新间隔设为 6 小时**（与仓库更新节奏一致）
2. 直接在 **🚀 节点选择** 里选择国家/地区
3. 每个国家组会自动使用该国延迟最低的节点
4. 免费节点波动大，失效节点会被客户端测速自动跳过

## 本地运行

项目使用 [uv](https://docs.astral.sh/uv/) 管理 Python 依赖：

```bash
uv sync --frozen
uv run --frozen python scripts/aggregate.py
```

## 上游源

默认采用质量优先的源：

- [onlyactions/proxy-scanner](https://github.com/onlyactions/proxy-scanner)：真实隧道测活并验证出口 IP
- [zueston/proxy-hub](https://github.com/zueston/proxy-hub)：真实代理测速后输出
- [shifeiquan520/shifeiquan3](https://github.com/shifeiquan520/shifeiquan3)：自动测活
- [snakem982/proxypool](https://github.com/snakem982/proxypool)：Proxypool 输出
- [0xRadikal/Free-v2ray-Configs](https://github.com/0xRadikal/Free-v2ray-Configs)：多轮验证
- [nodesradar/nodesradar-en](https://github.com/nodesradar/nodesradar-en)：每日测试
- [735754647/Free-Nodes](https://github.com/735754647/Free-Nodes)：Mihomo 连通性测试
- 土耳其节点额外从 `anaer`、`Barabama`、`MhdiTaheri` 中单独筛选，只保留 `TR` 节点

所有节点都必须先通过国家/地区识别，识别失败的节点不会发布。

少于 2 个节点的国家/地区组默认隐藏，避免客户端里出现大量单节点超时组。

### 土耳其补充

- [Barabama/FreeNodes](https://github.com/Barabama/FreeNodes)、[anaer/Sub](https://github.com/anaer/Sub)、[MhdiTaheri/V2rayCollector](https://github.com/MhdiTaheri/V2rayCollector)：只抽取 `TR` 节点，不接入这些池子的其他国家节点。

### 未默认接入的原始大池

- [Alirewa/V2ray-Configs](https://github.com/Alirewa/V2ray-Configs)：原始池数量大，但未做统一质量筛选，已从默认源移除。
- [vxiaov/free_proxies](https://github.com/vxiaov/free_proxies) 和 [awesome-vpn](https://github.com/awesome-vpn/awesome-vpn)：抽样延迟测试中超时比例很高，已从默认源移除。

## 当前统计

<!--STATS-->
更新于 **2026-11-27 01:00:14 北京时间**，共 **1889** 个去重节点

| 上游源 | 节点数 |
|---|---|
| onlyactions | 966 |
| zueston | 373 |
| shifeiquan | 136 |
| snakem982 | 41 |
| 0xRadikal | 116 |
| nodesradar | 26 |
| 735754647 | 212 |
| Barabama | 7 |
| anaer | 3 |
| MhdiTaheri | 9 |
<!--/STATS-->

## 声明

节点均来自互联网公开资源，仅供学习交流，请遵守当地法律法规。
