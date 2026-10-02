# 验证记录与边界

本文把项目记录中的历史验收和 2026-10-03 的本次文档审阅分开写。历史记录描述当时做过的动作；本次审阅只读本机源代码、安装 App 元数据和安全清单，没有把旧结果重新包装成新测试。

## 来源与时间

| 日期 | 来源 | 结论或检查范围 | 性质 |
| --- | --- | --- | --- |
| 2026-08-08 | 原始项目 `docs/PROJECT_HISTORY.md` | 0.1.12 / build 13 的安装、系统模式、helper、双风扇 automatic、菜单功耗刷新、详情/设置/退出路径和完整 21 项 Swift Testing 结果 | 历史本机验收 |
| 2026-08-20 | 原始项目 `docs/PROJECT_HISTORY.md` | 0.1.13 的 27 项 Swift Testing、候选包签名与文件检查；随后完成系统模式、菜单交互和正常退出恢复 | 历史候选与本机验收 |
| 2026-08-20 | 原始项目 `docs/PROJECT_HISTORY.md` | 0.1.14 / build 15 的版本与签名、单一主进程、helper `ping`、外部点击收起、菜单/详情/设置返回、右键菜单、正常退出恢复与重启后状态 | 历史本机验收 |
| 2026-10-03 | 本次只读审阅：本机源项目、安装 App、`Package.swift`、安全清单和测试文件 | 安装 App 显示 0.1.14 / build 15；`codesign --verify --deep --strict` 通过且签名为 ad-hoc；当前测试文件含 27 个用例声明；未重新构建、安装或运行完整测试 | 本次文档审阅 |

## 历史验收实际覆盖

根据项目沿革和安全清单，已明确记录的覆盖项包括：

- 菜单栏左键首页、右键菜单、详情返回、设置页和正常退出。
- 0.1.14 的 popover 外部点击收起和立即重开。
- 版本/build、App 严格签名、单一主进程、helper `ping`、helper 与共享协议兼容性。
- 在已记录的验收中，两枚风扇保持 `mode=0`，退出后恢复系统 automatic。
- 压力分与散热曲线解耦、90°C 紧急全速、RPM 硬件上下限、功耗输入边界和分级升速等核心逻辑测试。

## 当前仍不能声称已经验证的内容

项目记录明确保留了以下边界：

- 0.1.14 验收当轮没有主动切换实际风扇控制，因此不能把 0.1.14 的真实自动升降速曲线写成现场通过。
- 没有主动让电脑睡眠，也没有制造 heartbeat 或 XPC 断线；睡眠恢复和断线恢复路径保留在设计与测试边界中，但本次没有重新触发。
- 当前安装包为本机 ad-hoc 签名，未公证；它不是公开分发版本。
- 可见测试集中在核心计算和状态策略。UI、AppleSMC 真机写入以及特权 helper 的完整故障注入仍需要专门的授权现场验收。

## 后续验证范围

源码项目中的核心测试与构建入口可用于软件回归；硬件写入、helper 故障注入、睡眠与断线恢复应在目标设备上分别验收。构建与核心逻辑测试不能替代这些真实设备检查。

本仓库不提供可运行源码；本次没有执行应用构建、安装或 SMC 操作。

## 可核对的原始来源

相对原始项目根目录：

- `README.md`：当前版本、功能和开发入口。
- `docs/PROJECT_HISTORY.md`：按版本记录的历史决策与验收日期。
- `docs/SAFETY_AND_RELEASE.md`：发布和安装安全清单。
- `Package.swift`：macOS/Swift 版本、模块、系统框架和测试 target。
- `Sources/WannaFlyHigh/WannaFlyHighAppDelegate.swift`：菜单栏、popover 和外部点击监听。
- `Sources/WannaFlyHigh/AppModel.swift`：采样循环、功耗可见性和控制状态。
- `Sources/WannaFlyHighHelper/HelperService.swift`：连接校验、heartbeat 和 automatic 恢复。
- `Tests/WannaFlyHighCoreTests/CoolingPolicyTests.swift`：压力、曲线、RPM、功耗和分级升速测试。
