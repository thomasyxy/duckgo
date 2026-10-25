# LuwuDynamics 资料整合与仓库分析

核查日期：2026-10-25。范围：用户提供的 MakerWorld、GitHub、产品 Wiki、XGO-Duck 官网，以及这些入口直接指向的公开资料。

本报告整合公开资料与源码静态核查结果，不代表已经完成硬件装配、编译、烧录、实机测试或模型性能验证。版本、售价、链接状态均为本次核查快照。

## 1. 核心结论

- GitHub 账号公开仓库共 23 个，4 个明确属于 XGO-Duck，另有 4 个历史仓库已归档。
- XGO-Duck 已形成“硬件与打印件 → 强化学习训练 → ONNX 策略 → 板端运行 → 网页控制”的资料链路。个人复刻优先阅读 Arduino Uno Q 路线。
- 鸭子 Wiki 当前显示 0 篇文档，不能作为完整操作手册；官网、硬件仓库和 Arduino 运行仓库才是主要入口。
- MakerWorld 上传页列出 7 个模型，主页显示数量为 6，存在展示差异。多个详情页受到安全验证或 HTTP 403 限制，未确认完整下载清单或平台许可证。
- “公开源码/模型”和“许可完整、可自由商用”不是同一件事。鸭子硬件授权仍待定稿，两套运行仓库未发现顶层许可证；强化学习代码的 Apache-2.0 不能自动覆盖所有设计及模型资产。
- 最高优先级硬件风险：旧 ArduinoUnoQ PCB 的 U3/U4/U5 舵机接口 GND 与 SIGNAL 接反，必须断电核对实际板卡。
- 训练仓库、Arduino 运行端和 Mosaico 运行端并非同步发布；模型维度相同也不能证明观测语义、关节顺序和技能时序兼容。

### 当前工作区的辨析

当前 `/workspace` 是 `github.com/ma6174/duckgo`，用于 Go/XGo 函数与 DuckDB 标量 UDF 的包装和动态加载，并非 LuwuDynamics 机器人代码库。其 XGo 指 GoPlus 语言生态，不能与 XGO 机器人混淆。

依据：本地 `README.cn.md`、`go.mod`、`udf/`、`script/`。本次未修改现有源码，未运行构建或测试。

## 2. 四个入口如何使用

| 入口 | 主要价值 | 使用边界 |
|---|---|---|
| [GitHub](https://github.com/luwuDynamics) | 源码、训练任务、硬件工程、STL、装配文件、依赖与授权 | 应按仓库及文件分别判断许可，记录提交版本 |
| [产品 Wiki](https://wiki.xgorobot.com/) | 产品分类、XGO/RIG 教程、接口、系统镜像和固件入口 | Duck 文档尚未完善；其他机型参数不能套用 |
| [Duck 官网](https://www.xgorobot.com/duck.html) | 套件规格、购买边界、软件安装包、三大核心仓库 | 页面宣传动作不等于当前全部可部署技能 |
| [MakerWorld 中国站](https://makerworld.com.cn/zh/@LuwuDynamics) | 打印模型、部分打印配置、外观和社区说明 | 部分页面无法读取；未核实文件直链和许可证 |

## 3. 产品与代码对应关系

| 产品线 | 产品 | 主要代码/资料 | 注意事项 |
|---|---|---|---|
| XGO 四足 | lite3、mini2S | `luwu_os`、`xgo_doglib`、`luwu_mjlab`、`luwu-ros` | CM5 当前代际；历史 CM4/K210 资料要分开 |
| XGO 轮足 | mini2SW | `luwu_os`、`luwu_mjlab`、对应 Wiki | 比 mini2S 增加 4 个动力轮 |
| XGO 双轮足 | Rider2、Ranger | `xgorider2-firmware`、对应 Wiki/烧录入口 | Ranger 不应被烧录配置中的“六足”描述误导 |
| XGO 双足 | Duck | `xgoduck_hardware`、`xgoduck_rl`、两套 `xgoduck_runtime_*` | 独立链路，不应默认依赖 `xgolib` 或 Luwu-OS |
| RIG | Puppy、Hover、Arm、TARS | `rig_omni`、`rig_local`、`RIG-Muse` 等 | 国内/国际固件及账号服务需配套 |
| 跨产品工具 | 移动 App、MCP、Home Assistant | `xgo_android`、`xgo_ios`、`mcp-demo-browser`、`xiaolu_mcp_ha` | 需核对设备、服务与接口适用范围 |

Wiki 产品入口：[lite3](https://wiki.xgorobot.com/xgo/kb/zh-lite3)、[mini2S](https://wiki.xgorobot.com/xgo/kb/zh-mini2s)、[mini2SW](https://wiki.xgorobot.com/xgo/kb/zh-mini2sw)、[Rider2](https://wiki.xgorobot.com/xgo/kb/zh-rider2)、[Ranger](https://wiki.xgorobot.com/xgo/kb/zh-ranger)、[Duck](https://wiki.xgorobot.com/xgo/kb/xgo-duck)。

RIG 入口：[Puppy](https://wiki.xgorobot.com/kb/pet-puppy)、[Hover](https://wiki.xgorobot.com/kb/pet-hover)、[Arm](https://wiki.xgorobot.com/kb/pet-arm)、[TARS](https://wiki.xgorobot.com/kb/pet-tars)。

## 4. GitHub 全部公开仓库索引

以下“推送日期”为 GitHub API 的 `pushed_at`，按 UTC 日期记录，不等于稳定版本发布时间。许可摘要不能替代逐文件检查；“未明确”不等于禁止使用或不存在其他授权。

| 仓库 | 用途与主要技术 | 许可核查 | 推送/维护状态 |
|---|---|---|---|
| [.github](https://github.com/LuwuDynamics/.github) | 账号/组织介绍，`profile/README.md` | 未明确 | 2026-06-26 |
| [luwu-ros](https://github.com/LuwuDynamics/luwu-ros) | CM4/CM5，Ubuntu 26.04、ROS 2 Lyrical 镜像说明 | MIT；不覆盖整个镜像 | 2026-09-08；Early Alpha |
| [luwu_mjlab](https://github.com/LuwuDynamics/luwu_mjlab) | 四足训练；Python 3.11、mjlab 1.2.0、NVIDIA GPU | Apache-2.0 | 2026-08-14 |
| [luwu_os](https://github.com/LuwuDynamics/luwu_os) | CM4/CM5 教育系统；Python、Qt/PySide6、应用与库 | Apache-2.0 | 2026-08-29 |
| [mcp-demo-browser](https://github.com/LuwuDynamics/mcp-demo-browser) | Python、FastMCP、WebSocket 浏览器/计算器桥接 | 未明确 | 2026-07-07 |
| [RaspberryPi-CM4-XGO-Dog](https://github.com/LuwuDynamics/RaspberryPi-CM4-XGO-Dog) | 旧 CM4 机器狗控制、演示和网页图传 | MIT | 2025-08-26；已归档 |
| [RaspberryPi-CM4-XGO-Rider](https://github.com/LuwuDynamics/RaspberryPi-CM4-XGO-Rider) | 旧 CM4 双轮机器人程序、遥控和云语音 | MIT | 2025-08-27；已归档 |
| [RaspberryPi-CM5-XGO-Dog](https://github.com/LuwuDynamics/RaspberryPi-CM5-XGO-Dog) | CM5 四足控制及 AI 演示 | 未发现顶层许可证 | 2025-08-07；已归档 |
| [rdk-x3](https://github.com/LuwuDynamics/rdk-x3) | RDK X3、ROS 2 Humble/TROS、串口与机器人演示 | MIT | 2026-01-18 |
| [RIG-Muse](https://github.com/LuwuDynamics/RIG-Muse) | ESP32-S3 Muse 固件；ESP-IDF 6.0.1、服务 token | Apache-2.0 + NOTICE；服务另有条款 | 2026-10-06；实验性 |
| [rig_local](https://github.com/LuwuDynamics/rig_local) | ESP32 直连 ASR/LLM/TTS；自备 API Key | 未发现顶层许可证 | 2026-08-17 |
| [rig_local_web](https://github.com/LuwuDynamics/rig_local_web) | Flask、Vue 3/Vite、esptool-js；网页烧录及 NVS | 未明确 | 2026-08-16 |
| [rig_omni](https://github.com/LuwuDynamics/rig_omni) | Puppy/Hover/Arm/TARS 多板型；ESP32-S3、ESP-IDF 5.5 | README 声明 Apache-2.0；无顶层 LICENSE | 2026-10-01 |
| [rig_puppy](https://github.com/LuwuDynamics/rig_puppy) | 旧 Puppy 固件；ESP-IDF 5.4+、LVGL、ESP-SR | README 声明 MIT；链接的 LICENSE 不存在 | 2026-04-28；已归档 |
| [xgoduck_hardware](https://github.com/LuwuDynamics/xgoduck_hardware) | 鸭子 STL、PCB、BOM、打印与装配资料 | 授权范围尚待定稿 | 最近默认分支提交 2026-10-23 |
| [xgoduck_rl](https://github.com/LuwuDynamics/xgoduck_rl) | 鸭子 mjlab/PPO 训练、仿真与 ONNX 导出 | 代码 Apache-2.0；资产需另查 | 最近默认分支提交 2026-10-25 |
| [xgoduck_runtime_arduino](https://github.com/LuwuDynamics/xgoduck_runtime_arduino) | Uno Q Linux 推理、STM32 控制、网页 UI | 未发现顶层许可证 | 最近默认分支提交 2026-10-23 |
| [xgoduck_runtime_mosaico](https://github.com/LuwuDynamics/xgoduck_runtime_mosaico) | ESP-Mosaico/ESP32-S31；C 推理、LCD、网页控制 | 无顶层许可证；LCD 组件单独 Apache-2.0 | 2026-09-28；核查时仅一个提交 |
| [xgorider2-firmware](https://github.com/LuwuDynamics/xgorider2-firmware) | ESP32、PlatformIO/Arduino、WebSockets、FastLED | Apache-2.0 | 2026-10-12 |
| [xgo_android](https://github.com/LuwuDynamics/xgo_android) | Android BLE/Blockly；AGP 8.0.2、SDK 34 | 未明确 | 2024-10-31 |
| [xgo_doglib](https://github.com/LuwuDynamics/xgo_doglib) | Python 3.8+；`xgolib`、`xgoscreen` | MIT 徽章；无 LICENSE | 2026-01-19 |
| [xgo_ios](https://github.com/LuwuDynamics/xgo_ios) | Swift、Xcode/CocoaPods、iOS 13+ | 未明确 | 2025-01-02 |
| [xiaolu_mcp_ha](https://github.com/LuwuDynamics/xiaolu_mcp_ha) | Home Assistant 集成、HACS、WebSocket 服务 | Apache-2.0 + NOTICE | 2026-09-26 |

枚举来源：[GitHub 公开仓库 API](https://api.github.com/users/luwuDynamics/repos?per_page=100&type=public&sort=full_name)。鸭子四个仓库本次查询没有 GitHub Releases 和 Actions run 记录；这不代表官网没有独立发布安装包。

## 5. XGO-Duck 重点分析

### 5.1 官网确认的规格与功能边界

| 项目 | 已核实信息 |
|---|---|
| 产品形式 | 电子套件，外壳需自行 3D 打印，需自行组装 |
| 官网价格 | 2,328 元，页面标注原价 2,828 元；以购买时页面为准 |
| 主控 | Arduino Uno Q；Linux 执行策略推理，STM32 执行硬件控制 |
| 舵机 | 15 个飞特 1910 串口舵机：双腿 10、头颈 4、嘴部 1 |
| IMU | QMI8658 六轴；硬件仓库具体标为 QMI8658A |
| 电池 | 18650 电池组，容量不低于 2500mAh |
| 续航 | 官网称综合工况约 1 小时，未独立测试 |
| 频率 | 策略推理 50Hz，控制循环 100Hz；不是行走速度 |
| 控制 | 浏览器遥控、舵机 ID 设置、零位校准、状态监测 |
| 官网当前明确动作 | 行走、倾倒后起身、手动触发约 4 秒的叼取 |
| 不包含 | 预装音视频、TOF、语音对话、视觉识别、测距避障、自主导航 |
| 未找到明确参数 | 整机尺寸、重量、最高行走速度 |

来源：[Duck 官网](https://www.xgorobot.com/duck.html)、[硬件 README](https://github.com/LuwuDynamics/xgoduck_hardware/blob/master/readme.md)。

官网当前接入能力和 GitHub 最新实现需分开：官网称坐站、踢球、翻滚仅有训练环境支持；Arduino 仓库已附带翻滚和轮滑相关模型/控制实现。代码存在不等于官网安装包已包含，也不等于实机安全和效果已验证。

### 5.2 硬件仓库

- `structure/`：打印 STL，是 MakerWorld 无法读取时的重要替代入口。
- `PCBA/`：Altium 原理图、PCB、采购与修订说明。
- `docs/`：打印、装配及维护资料。
- `Assembly_Guide.pdf`、`BOM.md`：组装指南与采购清单。
- `LICENSING.md`、`UPSTREAM.md`、`ROADMAP.md`：授权、上游来源、文档和验证缺口。

关键链接：[STL](https://github.com/LuwuDynamics/xgoduck_hardware/tree/master/structure)、[BOM](https://github.com/LuwuDynamics/xgoduck_hardware/blob/master/BOM.md)、[组装 PDF](https://github.com/LuwuDynamics/xgoduck_hardware/blob/master/Assembly_Guide.pdf)、[PCB 公告](https://github.com/LuwuDynamics/xgoduck_hardware/blob/master/PCBA/README.md)。

官网套件清单与仓库自采 BOM 的螺丝、轴承数量有差异；仓库链接的另一套电子套件注明不含 Uno Q。采购前先确认“官网整套”“第三方电子套件”还是“完全自采”，不要拼接不同清单。

### 5.3 训练仓库

`xgoduck_rl` 负责仿真、训练、任务定义和策略导出，主要目录为 `src/mjlab_microduck/`、`scripts/`、`tests/`、`vendor/bam/`。

核查到的核心依赖：Python 3.12、`uv`、CUDA GPU、`mjlab==1.3.0`、`warp-lang==1.12.0`、`torch==2.9.1`。锁文件中解析为 MuJoCo 3.10.0、MuJoCo-Warp 3.8.1、ONNX 1.22.0、ONNX Runtime 1.24.4。

依据：[pyproject.toml](https://github.com/LuwuDynamics/xgoduck_rl/blob/fa621fc77ad9a05d69da13270e86118bdd3603fe/pyproject.toml)、[uv.lock](https://github.com/LuwuDynamics/xgoduck_rl/blob/fa621fc77ad9a05d69da13270e86118bdd3603fe/uv.lock)、[训练说明](https://github.com/LuwuDynamics/xgoduck_rl/blob/fa621fc77ad9a05d69da13270e86118bdd3603fe/README.md)。

普通复刻可先使用预置策略，不必先购置训练 GPU。`luwu_mjlab` 是四足训练仓库，不是鸭子训练的前置依赖。

### 5.4 两套运行端

| 项目 | Arduino Uno Q | ESP-Mosaico |
|---|---|---|
| 仓库 | [runtime_arduino](https://github.com/LuwuDynamics/xgoduck_runtime_arduino) | [runtime_mosaico](https://github.com/LuwuDynamics/xgoduck_runtime_mosaico) |
| 运行方式 | Linux/Python/ONNX Runtime + STM32 固件 | ESP32-S31，模型转换为 C 数组编入固件 |
| 主目录 | `python/`、`sketch/`、`assets/`、`tools/` | `main/`、`components/`、`tools/`、`tests/` |
| 依赖 | Arduino App Lab、`arduino:web_ui`、NumPy 2.2.6、ONNX Runtime 1.22.1 | CMake >=3.22、ESP-IDF 6.1.x、目标 `esp32s31` |
| 策略接口 | 61 维观测、14 维策略动作；嘴部另控 | 61 → 512 → 256 → 128 → 14，ELU 网络 |
| 控制链路 | Linux 50Hz，MCU 100Hz；网页端口 9527 | 50Hz 推理，通过 UART 连接下位机 |
| 部署注意 | 需 Bridge 补丁、sudo 和 router 2Mbps 配置 | 固定网络结构限制；板载 BMI270 不进入策略观测 |

Arduino 的安装步骤会改动设备库缓存和 systemd/router 配置，应先阅读脚本，不宜在无关 Linux 主机上直接执行。

依据：[Arduino README](https://github.com/LuwuDynamics/xgoduck_runtime_arduino/blob/master/README.md)、[依赖](https://github.com/LuwuDynamics/xgoduck_runtime_arduino/blob/master/python/requirements.txt)、[MCU 配置](https://github.com/LuwuDynamics/xgoduck_runtime_arduino/blob/master/sketch/sketch.yaml)、[Mosaico README](https://github.com/LuwuDynamics/xgoduck_runtime_mosaico/blob/main/README.md)、[转换工具](https://github.com/LuwuDynamics/xgoduck_runtime_mosaico/blob/main/tools/export_policy.py)。

### 5.5 兼容性与风险

| 优先级 | 发现 | 建议 |
|---|---|---|
| 高 | 旧 PCB 的 U3/U4/U5 GND 与 SIGNAL 接反，U2 不在范围内 | 全部断电后按实际连通性核查，遵循官方公告；不确定时停止接线 |
| 高 | 硬件许可未定稿，两运行端无顶层许可证 | 分享、销售、代工或二次分发前逐项确认授权 |
| 高 | 训练新 Angle 翻滚策略需要累计 IMU 翻滚角，Arduino 当前 `obs[55:61]=0` | 不可只替换 ONNX；先适配观测与技能时序 |
| 中 | Arduino 翻滚控制时长 1.9 秒，训练说明为含站稳约 7 秒 | 对照任务/策略版本验证整个技能流程 |
| 中 | 两运行端行走模型哈希不同，不能凭 61/14 维接口判等价 | 固定模型 SHA-256 和输入语义，做数值与实机分阶段验证 |
| 中 | Mosaico parity 测试依赖外部参考目录和 Windows DLL | 当前仓库存在测试不代表能够直接完整运行 |
| 中 | 控制界面为局域网 HTTP，鉴权/TLS/公网安全未核实 | 仅放在可信局域网，不直接暴露公网 |
| 中 | 独立装配记录、完整打印配置、首次运动验证与模型卡仍有缺口 | 按硬件路线图补齐复现记录 |

依据：[PCBA 公告](https://github.com/LuwuDynamics/xgoduck_hardware/blob/master/PCBA/README.md)、[观测构造](https://github.com/LuwuDynamics/xgoduck_runtime_arduino/blob/master/python/rl_core.py)、[控制器](https://github.com/LuwuDynamics/xgoduck_runtime_arduino/blob/master/python/controller.py)、[Mosaico 模型清单](https://github.com/LuwuDynamics/xgoduck_runtime_mosaico/blob/main/main/policy_manifest.json)、[parity 测试](https://github.com/LuwuDynamics/xgoduck_runtime_mosaico/blob/main/tests/test_parity.py)、[路线图](https://github.com/LuwuDynamics/xgoduck_hardware/blob/master/ROADMAP.md)。

## 6. 3D 打印模型索引

名称来自 [MakerWorld 上传页](https://makerworld.com.cn/zh/@LuwuDynamics/upload)。详情读取结果不代表文件已下载或打印可行性已验证。

| 模型 | 官方详情 | 本次核查 |
|---|---|---|
| XGO-Duck：打造属于你的双足机器鸭 | [3098881](https://makerworld.com.cn/zh/models/3098881-xgo-duck-da-zao-shu-yu-ni-de-shuang-zu-ji-qi-ya) | 安全验证，详情/许可未核实；STL 可转查硬件仓库 |
| RIG-TARS：开源星际伙伴 | [2922425](https://makerworld.com.cn/zh/models/2922425-zhuo-mian-ji-ju-shen-zhi-neng-ke-huan-ji-qi-ren-ri) | 安全验证，详情/许可未核实 |
| RIG-Arm：开源操控平台 | [2608307](https://makerworld.com.cn/zh/models/2608307-zhuo-mian-ji-ju-shen-zhi-neng-ji-jie-bi-rig-armkai) | 安全验证，详情/许可未核实 |
| RIG-Hover：开源桌宠 | [2604398](https://makerworld.com.cn/zh/models/2604398-zhuo-mian-ji-ju-shen-zhi-neng-zhai-ju-ping-heng-ch) | 可读取描述及 2 个打印配置，许可未显示 |
| XGO-lite 调试标定架 | [2423116](https://makerworld.com.cn/zh/models/2423116-xgo-litediao-shi-biao-ding-jia) | 安全验证，详情/许可未核实 |
| XGO-mini 整备标定架 | [2423086](https://makerworld.com.cn/zh/models/2423086-xgo-mini-zheng-bei-biao-ding-jia) | 安全验证，详情/许可未核实 |
| RIG-Puppy：开源机器狗 | [1854911](https://makerworld.com.cn/zh/models/1854911-zhuo-mian-ji-ju-shen-zhi-neng-chong-wu-rig-puppyka) | 可读取描述及 1 个打印配置，许可未显示 |

通过环境 HTTP(S) 代理复查数字 ID 地址，7 项均返回 HTTP 403。本报告不声称已归档 MakerWorld 模型文件，也不推断其商用条款。

Wiki 的部分 RIG 资料指向 MakerWorld 国际站 `makerworld.com`；用户给出的中国站 `makerworld.com.cn` 应分别核查，不能默认下载权限和许可证展示完全一致。

## 7. 下载与开发资料速查

### 7.1 鸭子安装、硬件和训练

| 资源 | 链接 | 说明 |
|---|---|---|
| Windows x64 安装包 | [ZIP](https://downloads.xgorobot.com/xgoduck/duck-setup-latest-win32-x64.zip) | 官网一键烧录工具 |
| macOS Apple 芯片 | [ZIP](https://downloads.xgorobot.com/xgoduck/duck-setup-latest-darwin-arm64.zip) | ARM64 |
| macOS Intel | [ZIP](https://downloads.xgorobot.com/xgoduck/duck-setup-latest-darwin-x64.zip) | x64 |
| Linux x64 | [ZIP](https://downloads.xgorobot.com/xgoduck/duck-setup-latest-linux-x64.zip) | x64 |
| Linux ARM64 | [ZIP](https://downloads.xgorobot.com/xgoduck/duck-setup-latest-linux-arm64.zip) | ARM64 |
| 版本清单 | [releases.json](https://downloads.xgorobot.com/xgoduck/releases.json) | 本次标记当前版本 0.1.14；为安装工具清单，不等于模型/固件统一版本 |
| 下载目录和校验 | [目录](https://downloads.xgorobot.com/xgoduck/)、[SHA256](https://downloads.xgorobot.com/xgoduck/SHA256SUMS-latest.txt) | 下载后再做实际校验 |
| App Lab 优化版 | [Windows EXE](https://downloads.xgorobot.com/xgoduck/ArduinoAppLab.exe) | 官网明确标注“非官方·群友优化版” |
| 硬件入口 | [xgoduck_hardware](https://github.com/LuwuDynamics/xgoduck_hardware) | STL、BOM、PCB、组装 |
| 运行入口 | [xgoduck_runtime_arduino](https://github.com/LuwuDynamics/xgoduck_runtime_arduino) | 普通 Uno Q 路线 |
| 自训入口 | [xgoduck_rl](https://github.com/LuwuDynamics/xgoduck_rl) | 仿真、PPO、导出 |

“latest”地址会变化，复现时应保存版本号、下载时间和实际 SHA-256；本次未完整下载安装包。

### 7.2 其他 XGO/RIG 通用资料

| 资源 | 链接 | 适用提醒 |
|---|---|---|
| 官方资源库 | [common-resources](https://wiki.xgorobot.com/kb/common-resources) | 先选机型 |
| Python 控制教程 | [xgolib](https://wiki.xgorobot.com/xgo/kb/zh-lite3/cmv0cw60d000ftqij0lkdst39) | 参数范围按机型；不是已确认的 Duck SDK |
| 新版 SDK | [SDK 总览](https://wiki.xgorobot.com/xgo/kb/zh-lite3/xgo-lite3-sdk-overview) | `xgolib`、`xgoedu-luwuos`、`xgo-blockly-luwuos` |
| 串口协议 | [TTL UART](https://wiki.xgorobot.com/xgo/kb/zh-lite3/cmsra8x59001njilgfj2bsdso) | 对应 XGO 文档默认 115200/8N1，不可套用 Duck 的 router 2Mbps 配置 |
| ROS 2 | [集成教程](https://wiki.xgorobot.com/xgo/kb/zh-lite3/xgo-lite3-ros2-guide) | 按系统镜像、ROS 版本配套 |
| RIG 开发 | [TARS 教程](https://wiki.xgorobot.com/kb/pet-tars/cmtwvlwmm003k12zs8zkaq5j7) | ESP-IDF 及固件分支需核对 |
| RIG MCP | [MCP 说明](https://wiki.xgorobot.com/kb/the-rig-series-robot/rig_en_mcp) | AI 控制机器人与机器人调用外部工具是两个方向 |
| Luwu Imager | [Windows](https://downloads.xgorobot.com/luwuimager/LuwuImager-Setup-latest.exe)、[macOS](https://downloads.xgorobot.com/luwuimager/LuwuImager-Setup-latest.dmg) | 与 Duck 独立安装工具区分 |
| Luwu-OS 发布 | [Releases](https://github.com/LuwuDynamics/luwu_os/releases/tag/2.1.3) | 核查时 Latest 为 2.1.3，2026-08-29 发布 |
| 下位机烧录 | [烧录中心](https://tools.xgorobot.com/) | 按机型与地区选固件，当前配置无 Duck |
| XGO App | [App Store](https://apps.apple.com/app/id1587117964)、[Google Play](https://play.google.com/store/apps/details?id=com.luwu.xgo_robot&hl=zh)、[APK](https://www.xgorobot.com/XGO_latest.apk) | 需确认当前设备支持 |
| RIG App | [下载](https://agent.luwudynamics.ai/download) | Wiki 注明面向国际版固件 |
| URDF/MJCF | [模型资源页](https://wiki.xgorobot.com/kb/common-resources/d113a2240e5d7e6750ab25ae5) | 抽查 lite3 MJCF ZIP 返回 200，不代表所有附件均有效 |

### 7.3 资源质量与版本冲突

- Duck Wiki 的参数页面多数条目“待补充”，不得据此推断 CM5、ESP32 或其他产品参数。
- lite3、mini2S、Rider2、mini2SW 的 `v2026.09.18` STEP 附件，本次站内/CDN 核查返回 404；抽查 lite3 驱动板原理图也返回 404。说明页存在不等于附件可下载。
- RIG-Puppy 烧录配置标为 3.8.0，而对应 manifest 标为 3.7.0；应以实际固件、发布文件和设备回报交叉核对。
- Ranger 烧录配置描述“六足”，与当前 Wiki“双轮足”冲突，以实际机型资料为准。
- RIG 国内/国际固件和账号服务需配套，不能仅看相同版本号。

证据：[烧录配置](https://tools.xgorobot.com/api/config)、[Puppy manifest](https://tools.xgorobot.com/bin/RIG/puppy/manifest.json)、[区域说明](https://wiki.xgorobot.com/kb/the-rig-series-robot/rig_en_downloads)、[lite3 STEP 资源说明](https://wiki.xgorobot.com/kb/common-resources/dab2d824ba9849f1e7edff7d0)、[mini2S](https://wiki.xgorobot.com/kb/common-resources/d59265c3dcd87825a9adb8638)、[Rider2](https://wiki.xgorobot.com/kb/common-resources/d83223be4c1a389b80e3d49b8)、[mini2SW](https://wiki.xgorobot.com/kb/common-resources/d68666b21dca82a6def15eb65)、[原理图说明](https://wiki.xgorobot.com/kb/common-resources/dff4a9808b930f67cc4a0d240)。

## 8. 授权与上游来源

鸭子硬件仓库的 [LICENSING.md](https://github.com/LuwuDynamics/xgoduck_hardware/blob/master/LICENSING.md)明确说明：发布文件本身不构成完整再利用授权；维护者允许个人/教育复刻、商业用途另行授权的表述只是意向，不是最终替代许可证，文件范围和条款仍待确定。

项目基于 Pollen Robotics 的 [Microduck](https://github.com/pollen-robotics/microduck) 和 [microduck_rl](https://github.com/pollen-robotics/microduck_rl)，训练使用 mjlab 和 BAM。硬件 [UPSTREAM.md](https://github.com/LuwuDynamics/xgoduck_hardware/blob/master/UPSTREAM.md)承认逐文件来源、精确上游版本及设计权限记录仍不完整。

上游 microduck_rl 的 README 区分 Apache-2.0 代码与 BY-SA-NC 硬件设计。不能把代码的宽松许可推导为所有 STL、PCB、机器人模型或第三方设计均可商用，也不能反过来把整个项目统一标成“禁止商用”。

联系授权时建议列出文件路径、提交版本、使用方式、是否修改、是否再分发和生产数量，向 `hello@xgorobot.com` 确认；无最终条款的个人/教育复刻也应确认适用权限。

## 9. 建议的资料整合方法

这是后续资料库规划，本次没有下载完整源码、STL 或安装包，也没有创建以下子目录。

```text
LuwuDynamics/
  00-index/          产品、链接、版本与核查状态
  01-products/       XGO 与 RIG 分产品规格
  02-source/         仓库提交快照与构建说明
  03-hardware/       BOM、PCB 修订、装配与校准
  04-printing/       STL、打印参数与型号适配
  05-training/       任务、依赖、检查点与 ONNX 模型卡
  06-runtime/        安装、固件、接口与实机验证
  07-licenses/       文件级许可与上游归属
```

每条资料至少记录：产品、类别、标题、官方 URL、仓库/提交、版本、核查日期、访问状态、许可证状态、适用硬件、依赖和风险备注。不同产品的同名“XGO”“模型”“固件”不能自动合并。

每次部署建议固定硬件版本、训练提交、运行提交、模型 SHA-256、舵机 ID/关节顺序、观测归一化、控制频率和校准参数。

### 推荐执行顺序

1. 明确目标是个人复刻、应用开发、自训还是商业产品；先处理授权和采购范围。
2. 复刻 Duck 时先读 BOM、组装指南、PCBA 公告和 Arduino README；检查打印件与硬件修订。
3. 在可靠支撑、限流和可断电条件下，按官方流程完成单舵机通信、ID、零位与 IMU 核查，再开展动作测试。
4. 先使用与运行端配套的预置策略；自训前固定依赖和模型接口，勿混用最新训练分支与旧运行端。
5. 完成最小闭环后再增加新技能、Mosaico 移植、云端工具或批量生产。

## 10. 核查快照与未完成项

重点代码快照：

- 硬件：[`b5e9ff16a98f7bbe5c146aad29ad38f0dde30a89`](https://github.com/LuwuDynamics/xgoduck_hardware/commit/b5e9ff16a98f7bbe5c146aad29ad38f0dde30a89)。
- 训练：[`fa621fc77ad9a05d69da13270e86118bdd3603fe`](https://github.com/LuwuDynamics/xgoduck_rl/commit/fa621fc77ad9a05d69da13270e86118bdd3603fe)。
- Arduino：[`8837d5a3bc7cf3746f6da2b55c7b9191026b9838`](https://github.com/LuwuDynamics/xgoduck_runtime_arduino/commit/8837d5a3bc7cf3746f6da2b55c7b9191026b9838)。
- Mosaico：[`e2fc679cbc9cb217ad0be7119e21763790434ff6`](https://github.com/LuwuDynamics/xgoduck_runtime_mosaico/commit/e2fc679cbc9cb217ad0be7119e21763790434ff6)。

未完成：完整源码/二进制/模型归档、安装包实际 SHA-256 校验、全新环境编译、烧录、测试运行、硬件连通性检查、打印和装配验证、实机动作验证、完整安全审计、所有附件遍历以及法律意见。部分来源链接指向可变分支；长期留档应保存对应提交快照。
