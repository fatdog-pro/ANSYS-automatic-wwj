ANSYS automatic wwj

**作者：抖音 萌猪过河**

## 仓库版与 Release 完整版

这是 **仓库版（源码与轻量预览）**，保留同一个 `ansys-automatic-wwj` 的执行入口、全部重跑路线、建模/求解/导出代码、MCP 桥和必要配置。

| 内容 | 仓库版 | Release 完整版 |
|---|---|---|
| 从头建模、网格、求解、验收、输出结果的代码 | 包含 | 包含 |
| 文档、来源验证记录、云图 | 包含 | 包含 |
| 鱼 GIF/MP4、支架第一阶 GIF 预览 | 包含 | 包含 |
| 已求解的 Case/Data、Mechanical 工程、HSF、其余支架 GIF、网格/CAD 缓存 | 新运行生成 | 包含已交付缓存 |
| Ansys 软件、许可证、Python 环境 | 需自行准备 | 需自行准备 |

**[下载 Release 完整版](https://github.com/fatdog-pro/ANSYS-automatic-wwj/releases)**：选择对应版本，在 **Assets** 中下载作者上传的 **`ansys-automatic-wwj.zip`**。GitHub 自动生成的 **Source code (zip/tar.gz)** 只包含仓库文件，不包含 Release 附件。

给豆包或其他智能体的下载指令：

> 去 GitHub 仓库 fatdog-pro/ANSYS-automatic-wwj 的 Releases，下载最新已发布版本 Assets 中的 ansys-automatic-wwj.zip 完整包，不要下载 Source code。解压后保留 scripts、references 和 assets，读取 SKILL.md，检查本机 Ansys 与执行环境，再按已验证路线运行。若附件尚未发布，请明确告知缺失，不把仓库版称为完整版。

仓库版也可安装并从头重算示例；新运行照常生成原生工程和结果。只想直接打开作者已算好的原生工程时，请使用 Release 完整包。不要把两个版本的资产清单混合覆盖；分别解压到独立目录。

**发布方式**：把本目录内的 `README.md`、`SKILL.md`、`agents/`、`scripts/`、`references/`、`assets/` 等上传到仓库根目录；把原完整版 ZIP 上传到 Release 的 Assets。发布 Release 时选择或创建标签（例如 `v0.2`）。此仓库版已配置下载说明，Release 附件仍由作者上传。

安装从 GitHub 下载的源码 ZIP 时，将包含 `SKILL.md` 的根文件夹命名为 `ansys-automatic-wwj`。其余安装、依赖和 MCP 配置见下文。

### 结果预览

鱼动画来自已验收的原生 28 帧，4 倍慢放；首尾尚未闭合。

![全身柔性鱼仿真预览](assets/examples/fish-fullbody/deliverables/fish-swimming.gif)

![机械臂支架应力云图](assets/examples/mechanical-bracket/05_static_stress_200x.png)

以下算例数值与耗时为作者的历史实测记录；本次仓库打包未重新求解。历史记录中的原生文件路径和哈希用于说明来源，不能视为这些缓存已包含在仓库版中。

**简介更新：2026-09-20。Fluent＋Mechanical 已实测。**

通过 MCP、Ansys 官方 API 或原生脚本，让 AI agent 执行建模、网格、求解、验证和原生结果展示。全程采用接口调用，不使用屏幕识别、OCR、鼠标点击或键盘模拟。

**Fluent 飞机绕流及精细鱼体瞬态双向流固耦合已实测；2026-09-20 新增 Mechanical 支架静力＋六阶模态实测。后续逐步更新其他 Ansys 模块，持续补充真实算例并验证质量。** 精细刚身柔尾分支已从新几何、时间 0 完整重跑 20 步；轻量全身柔性分支完整重跑 28 步；其他产品路线与历史探索不代表当前版本已验证全部模块。

## 同一个 skill，持续更新

飞机、柔性鱼尾、全身柔性鱼和 Mechanical 支架均内置于 **ANSYS automatic wwj**，使用同一个 `$ansys-automatic-wwj` 入口。全身柔性鱼不是独立 skill；带有 `fullbody` 字样的 ZIP 是本 skill 的整包导出快照。

- [全身柔性鱼执行路线](references/fish-fullbody-fsi.md)
- [全身柔性鱼代码](scripts/fish_fullbody/)
- [全身柔性鱼原生结果与动画说明](assets/examples/fish-fullbody/README.md)

## Mechanical 展示例程（2026-09-20）

[机械臂支架：静力＋六阶模态](references/mechanical-bracket.md) 已通过本机分阶段新建、网格、求解、反力验收、保存重开与原生动画导出。45075 节点，两项纯求解约 33 秒；提供六个 1080p GIF。完整建模、启动和导出需额外时间。模型生成代码与记录属于本 skill；已求解原生工程及全部六个 GIF 随 Release 完整版提供。见 [结果与录屏顺序](assets/examples/mechanical-bracket/README.md)。教学演示网格，未做网格收敛研究；跨客户端尚未实测。

## 全身柔性鱼快速版（2026-09-20）

保留全身柔性、主动驱动、双向 FSI 和 **0.70 秒 / 28 步**。简化鳃盖与小鱼鳍，初始网格降至 23570 单元，并直接利用求解时保存的原生帧，省去逐帧重开和重复渲染。本机完整一键重跑实测 **20.22 分钟**，包含新建模到原生动画、全部验收和重开；其他机器需独立验收。**首尾仍未闭合。** 见 [固定运行路线](references/fish-fullbody-fsi.md)。

## 运行后自动交付结果（2026-09-21）

全身柔性鱼完整入口现在自动输出 **原生工程、位移/水侧压力云图、运动及守恒曲线、CSV、JSON 摘要、GIF 和 MP4**。agent 在对话中直接展示动画和关键结果图，附可下载文件。GIF/MP4 是原生 28 帧的 4 倍慢放，首尾尚未闭合。导出步骤独立实测 42.39 秒；原 20 分 13 秒完整基线不含此次新增媒体导出，新组合入口尚未从头重新计时。

已包含 [媒体与结果示例](assets/examples/fish-fullbody/deliverables/results.md)；重跑依赖和失败后单独补导出见 [固定流程](references/fish-fullbody-fsi.md#自动结果与对话动画2026-09-21)。

## 这个 skill 提供什么

- 面向 agent 的任务入口和产品选路说明：[SKILL.md](SKILL.md)。
- 已跑通的飞机示例代码及固定执行路线：重新建模、生成网格、设置物理场、实际求解、检查结果、保存并重开原生文件。
- 精细鱼体瞬态双向流固耦合固定路线：[鱼体 FSI](references/fish-fsi.md)，包含曲面鱼身、鱼鳍、眼部和鳃盖，使用 Fluent 内置结构求解器、动态网格及原生动画。
- 本地 MCP 作业桥，供支持本地 stdio MCP 的客户端调用 Python SDK。
- 原始算例的云图、收敛历史及验证记录；CAD、网格和 Case/Data 缓存随 Release 完整版提供。

默认运行会**在使用者电脑上重新计算整个示例**。复用的是代码和步骤，agent 无需重新摸索同一套接口；内置旧结果不会代替本次求解。

## 当前验证范围

| 项目 | 当前状态 |
|---|---|
| Fluent 飞机示例 | 来源求解、数值验收、原生显示及 Case/Data 重开通过；既有记录保留 |
| Fluent 精细鱼体 intrinsic FSI | 完整新启动专用 GUI重跑入口通过：1 进程、新几何、时间 0、20 步 / 0.5 s、逐步及 HDF 验收、原生重开、20 帧读取/有限播放 |
| Fluent 全身柔性鱼（本 skill 内置） | 新几何、t=0、0.70 s / 28 步、逐步和独立验收、28 原生帧、最终 Case/Data 重开；完整入口 20.22 分钟，首尾未闭合 |
| Mechanical 支架静力＋六阶模态 | 本机可见会话新建、求解、反力平衡、原生重开、六个 GIF 验证通过；无网格收敛研究，跨客户端未实测 |
| 会话启动与迁移 | 鱼例显式 attach 分支未在本版另做完整实测；本机并行新启动尚未验证成功；飞机入口边界见既有交付记录；豆包客户端未实测 |
| 其他 Ansys 模块 | 后续逐步更新和实测；鱼例未调用 Mechanical 或 System Coupling |

来源环境为 **Windows、Ansys Student 2026 R1 / Fluent 26.1.0、Python 3.12、PyFluent 0.42.1、Gmsh 4.15.2**。其他软件版本和操作系统需要另行验证。详细证据见 [交付验证](references/delivery-validation.md)。

## 安装与执行方式

### 1. 准备本机环境

需要本机已安装 Fluent、有可用许可证，并准备匹配版本的 Python 依赖。本包不包含 Ansys 安装程序、许可证或 Python 虚拟环境。

解压并保留完整的 `ansys-automatic-wwj` 文件夹，包含 `scripts/`、`references/` 和 `assets/`。不要只复制 `SKILL.md`。

- **Codex**：将完整文件夹放入用户技能目录，例如 Windows 的 `%USERPROFILE%\.codex\skills\ansys-automatic-wwj`，确认客户端能发现该技能。
- **豆包或其他 agent 客户端**：按客户端支持的技能格式导入，并保留本地完整文件夹。不能假定所有客户端都能直接导入 Codex 的文件夹格式。

### 2. 选择本地执行方式

| 客户端能力 | 使用方式 |
|---|---|
| 能运行本机终端 / Python | 可直接运行包内脚本，MCP 不是必需条件 |
| 支持本地 stdio MCP | 配置本包 `ansys_unified` 桥，通过工具提交和观察脚本作业 |
| 只能读取技能文本，不能执行本机程序 | 仅导入 skill 无法自动控制本机 Ansys |

Skill 提供操作流程和代码，终端或 MCP 提供执行能力。云端 Python 环境不能仅凭这份 skill 访问用户电脑上的 Ansys。

### 3. 配置 Python 依赖

以下是 **Windows PowerShell** 示例。`C:\AnsysWork` 是示例工作目录，请替换为本机有写入权限的纯英文路径；`$skillRoot` 改为实际解压位置。已有兼容环境可直接复用，无需重新创建。

```powershell
$skillRoot = "$env:USERPROFILE\.codex\skills\ansys-automatic-wwj"
py -3.12 -m venv C:\AnsysWork\venv
$ansysPython = 'C:\AnsysWork\venv\Scripts\python.exe'
& $ansysPython -m pip install -r "$skillRoot\assets\examples\aircraft\replay\requirements.txt"
```

若网格依赖和 Fluent SDK 位于不同环境，可通过 `--mesh-python` 指定含 Gmsh / NumPy 的 Python。依赖基线见 [requirements.txt](assets/examples/aircraft/replay/requirements.txt)。

### 4. 需要 MCP 时配置一次

对于固定 Fluent 示例，使用 `ansys_unified` 即可，无需一次配置包内全部 MCP 服务。它是**本包提供的桥**，不是 Ansys 官方发布的 MCP。

1. 给运行桥的 Python 安装已验证版本：`python -m pip install fastmcp==4.0.3`。这里的 `python` 必须是下一步 `command` 指向的解释器。
2. 修改 [assets/runtime.local.json](assets/runtime.local.json)：设置本机 `ansys_roots`、`python_runtimes.fluent`、`python_runtimes.general` 和允许写入的 `project_roots`；同步实际 edition 和硬件说明。Fluent 作业使用 `python_runtimes.fluent`，它可以与运行桥的 Python 不同。
3. 参考 [assets/mcp.local.json](assets/mcp.local.json)，在客户端添加 `ansys_unified`：
   - **传输方式**：本地 `stdio`。
   - **command**：安装了 FastMCP 的 Python 的绝对路径。
   - **args**：三个独立参数：`<完整 skill 路径>/scripts/bridge.py`、`--config`、`<完整 skill 路径>/assets/runtime.local.json`。
   - **env**：`PYTHONUTF8=1`、`PYTHONIOENCODING=utf-8`；2026 R1 的 `AWP_ROOT261` 指向本机 Ansys 的 `v261` 目录。
4. 先调用 `product_info("fluent")` 检查桥和配置，再按 [Fluent 固定流程](references/fluent.md) 提交一次完整重跑任务。

包内 `.local.json` 是作者电脑的配置，换电脑时必须修改路径。MCP 连通、库存检查通过不等于许可证或求解成功；豆包也不会自动继承 Codex 的 MCP 会话。详细步骤见 [MCP 接入与迁移](references/mcp-and-migration.md) 和 [桥工具说明](references/bridge.md)。

## 运行飞机流体仿真示例

### 交给 agent 运行

完成环境配置后，可直接发送：

> 使用 ANSYS automatic wwj，按内置 Fluent 飞机示例的固定脚本完整重跑一次。先检查本机环境，再重新生成几何和网格、求解、验证并保存 Case/Data，在 Fluent 原生窗口展示结果并保持打开。汇报本次运行的真实结果和文件路径。

Codex 中也可通过 `$ansys-automatic-wwj` 明确选择该技能。

### 直接运行脚本

以下接续前面的 `$skillRoot` 和 `$ansysPython`。示例安装路径需改为实际 Ansys `v261` 目录；也可用 `--fluent-exe` 指定 `fluent.exe`。

```powershell
$env:AWP_ROOT261 = 'C:\Program Files\ANSYS Inc\v261'
$runRoot = 'C:\AnsysWork\runs'

# 显示固定阶段，不创建模型、不启动 Ansys
& $ansysPython "$skillRoot\scripts\replay_fluent_aircraft.py" --plan

# 校验资产、依赖元数据、安装位置和已有会话，不启动 Ansys
& $ansysPython "$skillRoot\scripts\replay_fluent_aircraft.py" --check-only --run-root $runRoot

# 预检通过后，实际重新建模、网格、求解、验证并展示
& $ansysPython "$skillRoot\scripts\replay_fluent_aircraft.py" --run-root $runRoot
```

完整执行会启动专用 Fluent GUI，输出写入工作目录下新建的独立子目录。发现已有 Fluent 会话时，脚本会停止启动新实例；先核对并保存现有项目，再由用户决定关闭旧会话或另行适配。它不会自动覆盖或关闭现有工程。

运行期间查看阶段日志，不要重复提交同一个作业。通过 MCP 运行时，保持客户端和桥进程运行，直到作业结束。

### 运行产物

| 本次运行目录中的文件 | 用途 |
|---|---|
| `replay_summary.json` | 本次执行阶段、实际迭代和完成/失败状态 |
| `geometry/` | 新生成的 CAD、网格及网格检查 |
| `results/aircraft.cas.h5`、`results/aircraft.dat.h5` | Fluent 原生模型和求解数据；配套保留，可在 Fluent 中打开 |
| `results/validation.json` | 本次数值收敛、质量守恒和系数稳定性验收 |
| `records/reopen_verification.json` | 原生保存重开后的数值核对 |
| `records/stages/` | 各阶段执行记录和日志 |

预检不代替 SDK 实际连接和许可证验证；进程退出码为 0 也不代替仿真验收。以本次生成的记录判断是否成功。阶段失败时保留文件并报告具体原因，不自动无限重试。

## 示例内容与精度限制

默认算例是自建简化固定翼飞机：机身长 4 m、翼展 6 m，来流 **20 m/s**、迎角 **5°**，稳态不可压缩 SST k–ω 模型。

来源运行使用约 5.94 万四面体单元，第 747 步达到数值验收，升力约 **554.09 N**、阻力约 **63.03 N**。这些是旧算例的对照值；重新生成的网格、迭代次数和结果可以变化，必须重新验收。

![来源算例的 Fluent 原生压力云图](assets/examples/aircraft/results/aircraft_pressure.png)

本示例是完整 CFD 流程演示，采用无棱柱边界层的粗网格，尚无网格独立性或风洞对照。**数值收敛已验证，气动精度尚未验证**，不能直接作为真实飞机设计依据。模型、判据及限制详见 [飞机算例说明](references/aircraft-example.md)。

仅希望预览保存好的旧结果时，使用 `scripts/open_fluent_aircraft.py --view pressure`；这不会重新求解。改变来流、迎角或几何属于新的计算任务，需要修改设置并重新求解，默认重跑入口固定为上述工况。

<!-- fish-fsi-20260917 -->
## 运行精细鱼体瞬态流固耦合示例

模型是来流中的精细刚性前身规定平移和柔性尾部响应；前身无结构单元，不是全鱼弹性或自由游泳。仅柔性尾采用 Fluent standalone intrinsic FSI，流体有限体积与内置结构有限元双向耦合，没有调用 Mechanical 或 System Coupling。

**默认快速教学预览：** 本机完整重跑约 21.4605 分钟，其中求解和逐步检查约 17.4779 分钟。该时间是本次真实记录，不保证其他机器相同；分辨率和网格质量均低于工程精算要求。

对 agent 说：“使用 ANSYS automatic wwj，按精细鱼固定代码，从新几何、新网格及时间 0 完整重跑 20 步，验证并在 Fluent 原生窗口播放动画，保存本次 Case/Data 和记录。”

接续上文 `$skillRoot` 与 `$ansysPython`，安装依赖并预检：

```powershell
& $ansysPython -m pip install -r "$skillRoot\assets\examples\fish\replay\requirements.txt"
& $ansysPython "$skillRoot\scripts\replay_fluent_fish.py" --plan
& $ansysPython "$skillRoot\scripts\replay_fluent_fish.py" --check-only --run-root C:\AnsysWork\runs
```

实际测试的完整路线由入口新启动 **1 进程的专用 Fluent GUI 会话**：

```powershell
& $ansysPython "$skillRoot\scripts\replay_fluent_fish.py" --run-root C:\AnsysWork\runs
```

启动前处理好已有 Fluent 会话；入口发现已有求解器时会停止新启动。若显式使用 `--attach-server-info $serverInfo`，只可选择授权重建的专用会话，该会话内模型会被新网格替换；此 attach 分支未在本版另做完整实测。 Gmsh 与 SDK 分处不同环境时增加 `--mesh-python`。不要对同一会话重复提交作业，成功后窗口保持打开。

本次实际求解模型 31,668 单元、8,281 个 ASCII 有效节点；水流 0.05 m/s，刚性前身及完整尾根 1 mm / 2 Hz 驱动，dt = 0.025 s，20 步至 0.5 s。最大尾部总位移 **0.692624 mm**；这些是本次来源数据，重新运行需独立验收。参数、全部数值和限制见 [精细鱼固定路线](references/fish-fsi.md)。

初始网格及 20 个完成步共 21 次 Fluent 原生质量检查全部通过：最低正交质量 **0.00322893**（门槛 ≥ 0.001）。记录为 `records/mesh-quality-0000.json` 至 `mesh-quality-0020.json`，汇总见 [四项 FSI 证据](assets/examples/fish/records/fsi_evidence.json)；Gmsh SICN 不替代这项检查。

每步先验收残差和原生网格，再核对保存的流固界面/正体积/结构 J，通过后才推进下一步；20 份即时记录为 `records/checkpoint-audit-0001.json` 至 `checkpoint-audit-0020.json`，与最终汇总逐项及源 Case/Data 哈希一致。

- [最终 Case（完整版）](https://github.com/fatdog-pro/ANSYS-automatic-wwj/releases) + [最终 Data（完整版）](https://github.com/fatdog-pro/ANSYS-automatic-wwj/releases)：在 Fluent 的 Read Case & Data 中配对打开。
- [原生动画（完整版）](https://github.com/fatdog-pro/ANSYS-automatic-wwj/releases)：与同目录全部 20 个 HSF 保留；迁移后显式加载此 CXA，Case 内原动画存储目录可能指向源电脑。
- [最终验收](assets/examples/fish/records/validation.json)、[完整入口记录](assets/examples/fish/records/replay_summary.json)、[20 步 HDF 审计](assets/examples/fish/records/checkpoint-audit.json)、[网格身份](assets/examples/fish/records/refined-mesh-identity.json)。
- [运动/受力 CSV](assets/examples/fish/records/motion_history.csv)、[残差 CSV](assets/examples/fish/records/step_residuals.csv)、[依赖](assets/examples/fish/replay/requirements.txt)、[资源说明](assets/examples/fish/README.md)。

随包保留初始/最终 Case/Data、20 显示帧和逐步记录。来源 20 份中间场不随包分发；重新计算会在本次 `checkpoints/` 生成。最终 Data 或显示用 HSF 不能恢复全部中间数值场。

本版按用户要求采用快速教学预览：较粗网格、20 步一个周期；原生低正交质量警告保留在记录中，明确采用 0.001 而非标准 0.01 的网格门槛。结果只用于流程和运动展示，不作工程定量结论。 这是层流、粗四面体网格的教学演示；局部薄鳍可能只有一层厚度单元，未做边界层膨胀、网格/时间步独立性、周期稳定或生物实验验证，不能据此预测真实鱼推进性能。原生网格质量检查覆盖初始网格及全部 20 个完成步，几何和界面检查覆盖全部保存完成步，不覆盖未保存的内部迭代。Gmsh SICN 不能替代 Fluent 原生正交质量检查，质量门槛也不等于网格独立性。刚体运动 UDF 随包提供 C 源码；新运行需用目标 Fluent 内置编译器重新编译。随包仅含本例正式库的两个 Windows DLL；已实测同机新目录、新 Fluent 261 单进程读取时自动用内置编译器重新编译未改动的 C 源码，再加载结果，时钟、十项报告、网格/流场/FE 历史及刚体状态保持一致，没有初始化或求解。这不证明跨电脑或平台兼容；跨电脑可先使用独立 CXA/HSF 观察结果。

![精细鱼来源运行的 Fluent 原生压力场](assets/examples/fish/results/fish-water-pressure.png)

![20 步真实运动与水动力历史](assets/examples/fish/records/plots/fish_motion_forces.png)

[统计和原始数据索引](assets/examples/fish/records/plots/fish_history_statistics.json)。可选 `replay/plot_fish_history.py` 需要 Matplotlib；默认求解不依赖它，曲线不平滑或外推。
<!-- /fish-fsi-20260917 -->

## 文档与目录

```text
ansys-automatic-wwj/
├── README.md       面向使用者的安装、运行和状态说明
├── SKILL.md        面向 agent 的工作指令
├── agents/         Codex 展示信息
├── scripts/        MCP 桥、库存及飞机/鱼体示例入口
├── references/     产品路线、迁移步骤和验证说明
└── assets/         本机配置示例、注册表及飞机/鱼体代码和对照资产
```

- [Fluent 固定执行路线](references/fluent.md)
- [MCP 配置与豆包迁移](references/mcp-and-migration.md)
- [飞机模型与原始结果说明](references/aircraft-example.md)
- [交付验证及其边界](references/delivery-validation.md)
- [其他产品的接口路线](references/product-routes.md)

## 后续更新

当前更新到 Fluent 飞机与鱼体示例。后续逐步补充其他 Ansys 模块的实际算例、可重复脚本、原生展示与保存重开验证，并继续改进 Fluent 算例的质量。每个模块单独记录验证结果；接口路线、历史探索或一个 Fluent 耦合案例不代表其他求解器已验证。
