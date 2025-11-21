✅ 1. MSTP 英文扩展
MSTP = Multiple Spanning Tree Protocol

中文：多生成树协议

IEEE 标准：802.1s（后来合并到 802.1Q-2005 中）

✅ 2. MSTP 是什么（简单易懂版）

MSTP = 把多个 VLAN 绑定到不同的生成树实例（Instance），从而在二层网络实现负载分担 + 环路保护。

以前：

协议	特点
STP（802.1D）	全网只有 1 棵树 → 所有 VLAN 都走一条路 → 链路浪费
RSTP（802.1w）	更快，但仍然只有一棵树
MSTP（802.1s）	可以有多棵树（Instance），每棵树可承载不同的 VLAN

所以 MSTP = 解决 VLAN 网络无法负载均衡的问题。

✅ 3. MSTP 的核心概念（华为最常考）
① MST Region（区域）

一个 MSTP 区域必须有：

同样的 Region Name

同样的 Revision（版本号）

完全相同的 VLAN→Instance 对应表

否则交换机会认为你不是一个区域。

② Instance（实例）

每一个 MSTI（Instance）都是一个独立的生成树。

例如：

Instance	VLAN
MSTI 1	VLAN 10–20
MSTI 2	VLAN 30–40

可以让：

Instance 1 → 主链路 A

Instance 2 → 主链路 B

从而实现二层链路 负载均衡。

③ IST（Internal Spanning Tree）→ Instance 0

MSTP 内部还有一个特殊的：

Instance 0（MSTI 0）

又叫 CST/IST

所有 MST 域外的设备，只看到一棵树（IST），保证兼容 STP/RSTP。

✅ 4. MSTP 的工作原理（极简版）

第一步：确定 MST Region
交换机比对（Name、Revision、VLAN 分配表）
→ 一致 → 同区域
→ 不一致 → 作为单独区域看待

第二步：每个 MSTI 各自选根桥（Root Bridge）

不同实例可以选不同根桥。

第三步：每个实例独立计算生成树（根端口 / 指定端口）
从而做到：

MSTI 1 可能把某条链路阻塞

MSTI 2 可能把另一条链路阻塞

避免所有 VLAN 都堵在同一根链路上。

✅ 5. MSTP 的优点（华为考试常考）
① VLAN 负载均衡

比 STP/RSTP 多棵树，链路不再浪费。

② 与 RSTP 同级速度（快速收敛）

MSTP 基于 RSTP 的 BPDU，速度快。

③更适合大型网络（园区网/城域网）
✅ 6. 华为设备 MSTP 配置示例（最常用模板）
stp region-configuration
 region-name HUAWEI
 revision-level 1
 instance 1 vlan 10 20
 instance 2 vlan 30 40
 active region-configuration


设置根桥（例子）：

stp instance 1 root primary
stp instance 2 root secondary
