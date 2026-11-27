# CHANGELOG

## [2026-11-25 15:07] 编写 Shell 端口连通性检测脚本

- **需求/问题描述**：
  > 检测端口连通性，执行方式选择 Shell。

- **实际实现的功能与改动**：
  - 本地新增端口连通性检测脚本 `check_port.sh`：基于 `nc` 检测单个或多个 TCP 端口，支持 `-t` 指定超时秒数（默认 3 秒）、`-h` 查看帮助；在 macOS 下自动附加 `-G` 控制 TCP 连接超时；校验主机、端口范围（1-65535）与超时参数，失败时输出 `nc` 报错信息并返回非零退出码。
  - 语法与参数验证：通过 `bash -n check_port.sh` 语法检查，验证 `-h`、缺少参数、非法端口 `70000` 及非法超时 `0` 均按预期返回。
  - 真实端口检测验证：在本机启动真实 TCP 监听端口并分配未监听端口，验证可达端口返回退出码 `0` 与 `[OK]`、不可达端口返回退出码 `1` 与 `[FAIL]`、多端口批量检测返回混合结果与退出码 `1`。

- **涉及文件**：
  - `check_port.sh` (+79 / -0，仅保留在本地，不纳入远端仓库)
  - `docs/CHANGELOG.md` (+20 / -0)
  - `context/2026/11/25/15-07-50/对话.md` (+59 / -0)

- **Git 提交**：`1569c5b docs: record shell port check script task and archive conversation`

---

## [2026-11-27 20:31] 研究公网 IP 124.160.107.92

- **需求/问题描述**：
  > 研究这个IP124.160.107.92

- **实际实现的功能与改动**：
  - 归属与注册核查：通过 APNIC WHOIS、APNIC RDAP、IPinfo 与 ip-api 核实该 IP 属于中国联通浙江分公司（杭州滨江，`AS4837`），精确分配子网为 `124.160.107.88/29`（`CESHIHZ`，2010-09-08 注册），并核对相邻子网 `.80/29` 与 `.96/30` 的登记信息。
  - 路由与安全信誉核查：通过 RIPEstat 验证 `124.160.0.0/16` 在 `AS4837` 下的 BGP 可见性（321/322）与 RPKI ROA 状态（`valid`）；通过 Cloudflare DoH、HackerTarget、Shodan InternetDB、GreyNoise Community 与 DNSBL（SpamCop、Barracuda）完成反向解析、历史域名 `shaobo.tech` 现行状态、子网暴露面及安全信誉核查。
  - 实时连通性验证：执行 ICMP ping（3 次全超时）及 80/443 端口 HTTP/HTTPS 请求（均 5 秒超时），确认当前无常规公开 Web 响应。

- **涉及文件**：
  - `docs/CHANGELOG.md` (+18 / -0)
  - `context/2026/11/27/20-31-00/对话.md` (+52 / -0)

- **Git 提交**：待提交

---
