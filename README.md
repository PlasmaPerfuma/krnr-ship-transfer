# KRNR Ship Transfer（海军舰船转移）

Kaiserreich / KR 海军重置（Kaiserreich Naval Rework）的外交舰船转移子模组。
在外交界面新增一个**仅玩家可用**的行动，可以按舰种和数量把舰船转移给其他国家，也支持按舰名指定单艘舰船。

> A player-only diplomatic action for Kaiserreich + Kaiserreich Naval Rework that transfers ships
> to another country by type & amount, or by renaming a ship.

---

## 依赖 / Requirements

| 模组 | 说明 |
|---|---|
| Kaiserreich | 必需 |
| Kaiserreich Naval Rework（KR海军重置） | 必需，提供维修舰/支援舰等舰种 |
| Hearts of Iron IV 1.19.* | `supported_version="1.19.*"` |

启动器加载顺序建议：Kaiserreich → Kaiserreich Naval Rework → 本模组。

## 安装 / Installation

1. 把 `KRNR_Ship_Transfer_Demo` 文件夹放到：
   `Documents\Paradox Interactive\Hearts of Iron IV\mod\`
2. 把 `KRNR_Ship_Transfer_Demo.mod.example` 复制为
   `Documents\Paradox Interactive\Hearts of Iron IV\mod\KRNR_Ship_Transfer_Demo.mod`
   并把它里面的 `path=` 改成你机器上第一步那个文件夹的**实际绝对路径**
   （Windows 路径用正斜杠 `/`）。
3. 完全退出并重新打开 Paradox 启动器，在播放集里启用本模组。

文件夹内部的 `descriptor.mod` 不用改、也不要移动到别处。

## 使用 / Usage

1. 打开某个国家的外交界面，在最上面找到 **海军舰船转移**。
2. 用每个舰种右侧的圆形按钮选择数量：
   - 单击 `+` / `-`：±1
   - `Ctrl` + 单击：±5
   - `Shift` + 单击：±10
   - 数量不会低于 0
3. 窗口底部有三个按钮：

| 按钮 | 行为 |
|---|---|
| 随机舰船 | 选项按钮。点一次选中（按钮显示按下状态），再点一次取消 |
| TANGO 指定舰 | 选项按钮同上。选中后只转移改名为 `TANGO` 的那一艘舰船 |
| 重置选择 | 把所有数量和模式清回初始状态 |

两个模式按钮**互斥**，且默认都不选中（= 按所选舰种与数量转移）。
4. 点击窗口底部的 **递交** 执行转移；点 **取消** 不执行。

### TANGO 指定舰

1. 在舰队列表里把目标舰船改名为 `TANGO`（大小写要与脚本一致）。
2. 选中 **TANGO 指定舰** 模式。
3. 只在**一个**舰种上设置数量 1（多个舰种同时有数量时，只会处理第一个匹配的舰种）。
4. 点击 **递交**。

## 实现说明 / How it works

| 文件 | 作用 |
|---|---|
| `common/scripted_diplomatic_actions/krnr_ship_transfer.txt` | 外交行动本体、`send_scripted_gui`、实际转移逻辑 |
| `common/scripted_guis/krnr_ship_transfer.txt` | 按钮效果与元素可见性（模式按钮的按下状态） |
| `common/scripted_effects/krnr_ship_transfer_effects.txt` | 初始化计数器变量 |
| `common/on_actions/krnr_ship_transfer_on_actions.txt` | 开局与每日兜底初始化（兼容旧存档） |
| `interface/krnr_ship_transfer.gui` | 窗口布局 |
| `localisation/` | 中文 / 英文本地化 |

要点：

- 状态保存在发起国的国家变量里：`krnr_sub_qty`、`krnr_dd_qty`、`krnr_cl_qty`、`krnr_ca_qty`、
  `krnr_bc_qty`、`krnr_bb_qty`、`krnr_cv_qty`、`krnr_support_qty`、`krnr_mode`
  （`krnr_mode`：0 = 一般转移，1 = 随机舰船，2 = TANGO 指定舰）。
- 数量文本用 `[?ROOT.krnr_*_qty]`：在 `context_type = diplomatic_action` 的脚本 GUI 里，
  **ROOT 才是看到窗口的那个国家**，不带前缀的地数据文本读的是上下文默认作用域（不是国家），
  这正是早期"数字不刷新"的原因。
- 窗口**没有**设置 `dirty`：HOI4 默认每个 tick 刷新脚本 GUI，设了 `dirty` 反而只在
  该变量变化时刷新。
- 中文本地化文件必须保存为**带 UTF-8 BOM**，否则游戏读不到中文（会显示键名）。

## 已知问题 / Known issues

- `logs/error.log` 可能出现
  `Unexpected token: context_type ... common/scripted_guis/krnr_ship_transfer.txt`。
  这是解析器的告警，实测不影响窗口、按钮与转移。
- 目前 **随机舰船** 与默认模式底层行为相同（都按所选舰种+数量逐艘转移）。
- 支援舰使用 `support_ship` 类型；KR 的维修舰是独立类型（参考 Ship Transfer Tool 的 `repair_ship`），
  本模组尚未单独提供维修舰按钮。

## 授权 / License

本仓库只包含本模组自身的脚本与本地化，**不包含** Kaiserreich、Kaiserreich Naval Rework
或任何第三方模组的文件。使用或再发布前请遵守这些模组各自的授权。
