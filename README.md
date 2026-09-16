
# ANSYS automatic wwj

**作者：抖音 萌猪过河**

通过 MCP、Ansys 官方 API 或原生脚本，让 AI agent 执行建模、网格、求解、验证和原生结果展示。全程采用接口调用，不使用屏幕识别、OCR、鼠标点击或键盘模拟。

**目前更新至 Fluent，已完成简化三维飞机流体仿真示例实测。后续将逐步更新其他 Ansys 模块，持续补充真实算例并验证各模块质量。** 包内其他产品的接口路线和历史探索记录，不代表当前版本已验证全部模块。

## 这个 skill 提供什么

- 面向 agent 的任务入口和产品选路说明：[SKILL.md](SKILL.md)。
- 已跑通的飞机示例代码及固定执行路线：重新建模、生成网格、设置物理场、实际求解、检查结果、保存并重开原生文件。
- 本地 MCP 作业桥，供支持本地 stdio MCP 的客户端调用 Python SDK。
- 原始算例的 CAD、网格、Case/Data、云图、收敛历史及验证记录，供对照或明确要求的预览。

默认运行会**在使用者电脑上重新计算整个示例**。复用的是代码和步骤，agent 无需重新摸索同一套接口；内置旧结果不会代替本次求解。

## 当前验证范围

| 项目 | 当前状态 |
|---|---|
| Fluent 飞机示例 | 来源运行完成真实求解、数值验收、原生显示和 Case/Data 重开核对 |
| 固定重跑入口 | 已检查格式、语法、只读预检，并独立重新生成和验证几何/网格；打包时没有通过新入口再次完成整轮 CFD 求解 |
| 其他 Ansys 模块 | 后续逐步更新和实测；已有路线不等于完成质量验证 |
| 豆包接入 | 提供迁移说明，尚未在豆包客户端完成接入实测 |

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

## 文档与目录

```text
ansys-automatic-wwj/
├── README.md       面向使用者的安装、运行和状态说明
├── SKILL.md        面向 agent 的工作指令
├── agents/         Codex 展示信息
├── scripts/        MCP 桥、库存和飞机示例入口
├── references/     产品路线、迁移步骤和验证说明
└── assets/         本机配置示例、注册表及飞机代码/对照资产
```

- [Fluent 固定执行路线](references/fluent.md)
- [MCP 配置与豆包迁移](references/mcp-and-migration.md)
- [飞机模型与原始结果说明](references/aircraft-example.md)
- [交付验证及其边界](references/delivery-validation.md)
- [其他产品的接口路线](references/product-routes.md)

## 后续更新

当前更新到 Fluent 飞机示例。后续将逐步补充其他 Ansys 模块的实际算例、可重复执行脚本、原生展示与保存重开验证，并按实测情况更新支持范围。每个模块单独记录验证结果，不把接口列表当作已完成的功能清单。
