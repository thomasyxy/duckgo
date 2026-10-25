# XGO-Duck 组装教程与检查表

适用版本：Arduino Uno Q + XGO-Duck 扩展板 + 15 个飞特 1910 总线舵机。不是 ESP-Mosaico 版本，也不适用于 XGO 四足/RIG。

核查日期：2026-10-25。硬件提交 `b5e9ff16a98f7bbe5c146aad29ad38f0dde30a89`，运行端提交 `8837d5a3bc7cf3746f6da2b55c7b9191026b9838`。

本教程整合官方资料与当前代码行为，未经过独立实机装配验证。机械方向、轴承位置、走线和外壳装配必须同时对照 [官方 25 页装配图](computer:///workspace/XGO-Duck-官方装配图.pdf)。以下 PDF 页码与页脚一致。

## 先看这五条

1. **先烧录和逐个编号，再机械装配。** 每次只接一个舵机，避免重复 ID 一起被改号或设零。
2. **旧板 U3/U4/U5 的 GND 与 SIGNAL 接反。** 所有电源断开后核查实际连通关系；不能仅按照片、线色或下载日期判断。
3. **校准与卸载时必须支撑住机器人。** 未上扭矩的关节会松开，整机可能倒下；手指不要放在关节夹点。
4. **不同页面的螺丝说明有冲突。** 本教程按当前 PDF 标注，未确定的规格和数量单独列出，不用“差不多”的螺丝替代。
5. **关网页、松摇杆不是卸载急停。** `Zero motion` 只清零行走指令；卸载使用 `Torque off`，无响应时安全切断执行器供电。

## 总体顺序

备料与打印 → PCB/供电检查 → 烧录系统 → 15 个舵机逐个设 ID → 按 PDF 装底座/双腿/头部 → 安装电池与控制板 → 全部在线检查 → 零位校准 → 支撑下默认姿态 → 地面小幅试走。

首次组装不要加入轮滑附件、翻滚训练或自行替换 ONNX 模型。

## 第 1 步：备料

### 基础版采购清单

以下是仓库 BOM 的采购数量，不是精确的实装消耗数。购买官方整套时还应核对随箱清单；第三方电子套件可能不含 Uno Q。

| 类别 | 物品 | 数量/要求 |
|---|---|---|
| 控制板 | Arduino Uno Q | 1，2GB 或 4GB |
| 扩展板 | XGO-Duck 扩展板 | 1，核对 PCB 修订与器件焊接 |
| 舵机 | 飞特 1910 总线舵机 | 15；文档确认 HD-1910-C001/HD1910M |
| 线缆 | AMP 三针线缆 | 15，接口及极性按实物核对 |
| 电源 | 18650 电池组 | 1；BOM 要求容量 >2500mAh、放电倍率 >3C、XH2.54 接口 |
| 充电器 | 8.4V 锂电充电器 | 1，DC5521 插头；极性/兼容性需确认 |
| 自攻螺丝 | M2×6 沉头、平尾 | 200 |
| 自攻螺丝 | M2.3×8 平顶圆头、平尾 | 6 |
| 机丝螺丝 | M3×16 圆头 | 4 |
| 轴承 | 内径10×外径15×宽3mm | 2 |
| 轴承 | 内径16×外径22×宽4mm | 11 |
| 粘接材料 | 双面胶 | 嘴部软垫使用；型号、厚度和用量未明确 |
| 打印件 | 刚性结构及 TPU 软垫 | 见下一节 |

工具建议：匹配螺丝的螺丝刀、万用表、镊子、舵机编号标签、零件分类盒、USB-C 数据线、稳定支撑架及合适的限流保护设备。工具选择属于整理建议，不是官方套件承诺。

**采购前确认：** PDF 第 14 页另标 M2.3×4，注明为副舵盘自带螺丝；BOM 没有单列。第 11 页 M2.3×8 没写实装数量。供应商所称输出轴螺丝 M2.5×6 不能直接当作 M2.3×8 替代品。

来源：[BOM](https://github.com/LuwuDynamics/xgoduck_hardware/blob/b5e9ff16a98f7bbe5c146aad29ad38f0dde30a89/BOM.md)、[器件说明](https://github.com/LuwuDynamics/xgoduck_hardware/blob/b5e9ff16a98f7bbe5c146aad29ad38f0dde30a89/docs/BOM.md)。

## 第 2 步：打印并检查配合

从 [当前 STL 目录](https://github.com/LuwuDynamics/xgoduck_hardware/tree/b5e9ff16a98f7bbe5c146aad29ad38f0dde30a89/structure)下载，保留文件原名。不要混用不同版本的左右件。

| 打印数量 | 文件名 |
|---|---|
| 每个文件 2 件，刚性件 | `2x_ankle_axis.STL`、`2x_hip_rol_output.stl`、`2x_hip_rol_shell.stl`、`2x_hip_yaw2rol.STL`、`2x_neck.stl`、`2x_shank.stl`、`2x_thigh_support.stl` |
| 2 件，TPU | `2x_foot_bottom_tpu.stl` |
| 每个文件规划 1 件，刚性件 | `battery.stl`、`body.stl`、`body_left_shell.stl`、`body_right_shell.stl`、`eye.stl`、`eye_shell.stl`、`head.stl`、`head_pitch.stl`、`head_servo_support.stl`、`head_shell.stl`、`head_yaw.STL`、`hip_yaw_support.STL`、`jaw.stl`、`left_foot.stl`、`right_foot.stl`、`thigh_left_shell.stl`、`thigh_right_shell.stl` |
| 每个文件 1 件，TPU | `mouth_buttom_tpu.stl`、`mouth_up_tpu.stl` |

按文件名规划，本体共 35 件；这不是已经完成实装核销的清单。`mouth_buttom_tpu.stl` 是仓库原拼写。旧打印文档中的 `hip_yaw2rol.STL` 链接失配，应查当前 `2x_hip_yaw2rol.STL`。

1. 先试打一个代表性的舵机安装位和轴承座，确认能装入且不过松，不要强压轴承或靠螺丝强拉变形。
2. 清理支撑残留，核对左右脚、左右壳体和线缆通道。
3. 再打印其余结构；脚底和嘴部文件明确使用 TPU。
4. 按 PDF 分阶段摆放零件，记录材料和切片参数。

**参数边界：** README 给出 4 道墙、13% Gyroid 及槽形支架的摆放参考，但没有明确对应全部 STL；不能当作全套统一配置。文档记录过 PLA/A1/0.4mm 喷嘴/0.2mm 层高的本地 beta 预设，仓库未提供可复核的完整 3MF。温度、速度、支撑、公差补偿和 TPU 硬度仍需自行验证，不在此编造数值。

可选轮滑附件为 `2x_tire_support.STL`×2、`4x_tire.STL`×4、`4x_tire_axis.STL`×4；当前 PDF 没有完整轮滑装配步骤，首次组装先跳过。

来源：[PRINTING.md](https://github.com/LuwuDynamics/xgoduck_hardware/blob/b5e9ff16a98f7bbe5c146aad29ad38f0dde30a89/docs/PRINTING.md)、[README 打印说明与方向图](https://github.com/LuwuDynamics/xgoduck_hardware/blob/b5e9ff16a98f7bbe5c146aad29ad38f0dde30a89/readme.md)。

## 第 3 步：检查电路板和供电

**这一步全部断电：拔掉电池、USB 和充电器。**

1. 检查焊接、器件方向、松动端子和明显短路；自制板应由具备经验的人员对照原理图审查。
2. 确认 U3/U4/U5 接口属于哪一修订，按焊盘编号和万用表测量确认，不凭线色推断。
3. 确认电池接口 CN1 的正负、电池保护配置、充电器极性和兼容性。不确定时停止，不试插上电。
4. 舵机供电网络为 `VCC8.4V`；不要把 8.4V 接到 USB、5V 或逻辑信号引脚。
5. 单个舵机首次供电应具有合适的限流保护；不要先把整机 15 个舵机全部接上试错。

| U3/U4/U5 工程焊盘 | 已确认旧版 | 2026-10-02 修订版 |
|---|---|---|
| 1 | GND | SIGNAL |
| 2 | VCC8.4V | VCC8.4V |
| 3 | SIGNAL | GND |

焊盘编号不是任意观察方向的左、中、右。对**确认匹配旧版映射**的板，官方处理办法是在板端一侧交换 GND/SIGNAL 的端子，电源线不动，或使用专用交叉转接线。不能两端都交换、强行反插或短接 GND 与 SIGNAL。切换到修订板时恢复标准线束。

完整操作与测试要求以 [PCBA 安全公告](https://github.com/LuwuDynamics/xgoduck_hardware/blob/b5e9ff16a98f7bbe5c146aad29ad38f0dde30a89/PCBA/README.md)为准。本教程不提供未经验证的割线点或电池插头正负示意。

通信说明：舵机总线 D0/D1、1Mbps；IMU QMI8658 使用 D20/D21；Linux/MCU Bridge 的 2Mbps 是另一条链路。使用配套扩展板并对照其工程，不根据普通 UNO 的引脚布局自行替代。

## 第 4 步：烧录与联网

### 推荐：官网 Duck Setup 完整系统烧录

**会清除板上原系统和用户数据，请先备份。** 本次安装软件为 0.1.14，默认系统镜像为 Duck OS 0.1.0-rc6，两者不是同一版本。镜像目录将 rc6 标为候选版本，尚需板卡验收。

1. 从 [官网软件下载区](https://www.xgorobot.com/duck.html#software)下载对应系统和处理器架构的 Duck Setup，解压并打开。
2. 点击 `下载镜像`，等待下载和 SHA-256 校验；也可选择与目录大小/哈希匹配的本地镜像。
3. 断开 USB、电池及其他电源。
4. 对照安装器图与 [Arduino 官方 EDL 图](https://docs.arduino.cc/software/app-lab/configure/flash/#step-1-set-your-board-to-edl-mode)，在指定 EDL 两针安装跳线帽。**不能只靠口头位置描述短接，也不是按复位按钮。**
5. 保留跳线，用 USB-C 数据线只连接一块待烧录板；勾选 `已按图连接一块板子`，点击 `开始安装`，再确认。
6. 等待完整写入并报告成功。写入时不拔线、不断电，不只凭进度条判断。
7. 成功后先断开全部电源，再取下 EDL 跳线帽。
8. 重新接 USB，等待系统启动和识别；首次可能需几分钟，电源灯亮不代表系统就绪。
9. 进入 `配置 XGO-Duck`，连接 Wi-Fi 或设置 Duck 热点，让控制设备与机器人处在同一网络。
10. 启动 App，访问 `http://实际设备IP:9527/`。`通过 USB 访问`只适用于当前电脑且需要保持 USB。

烧录 EDL 时按工具/原厂流程使用板卡，不连接电池和舵机负载。正常舵机调试所需供电另按已核实的扩展板接法准备；不要假定电脑 USB 能承担整机负载。

### 可选：已有系统，App Lab 部署项目

已有合适系统且愿意手动部署时，阅读 [runtime README](https://github.com/LuwuDynamics/xgoduck_runtime_arduino/blob/8837d5a3bc7cf3746f6da2b55c7b9191026b9838/README.md)。这不是完整系统烧录步骤。

App Lab 打开项目并下载板卡库后，停止 App，在 **UNO Q 本机的项目目录**执行：

```bash
python3 tools/prepare-bridge.py
sudo bash tools/router-2m.sh
```

然后回 App Lab 点击 `Run`，访问设备的 9527 端口。脚本会修补库缓存及 router/systemd 配置，不要在无关电脑上直接执行；库缓存被替换后可能要重新准备。

官网的 `ArduinoAppLab.exe` 标注为“非官方·群友优化版”，不是 Arduino 官方发行版。

## 第 5 步：逐个设置 15 个舵机

此时不要把全部舵机并到总线上。舵机未安装受力构件，轴处于卸载状态；改变线缆前先断电。

每个舵机重复以下流程：

1. 只连接一个舵机，按核实过的接口安全上电。
2. 打开 `Servo setup`，点 `Scan all IDs`，选择扫描到的舵机。新舵机可能是 ID1，以实际扫描为准。
3. 卸载状态下将轴摆到需要的机械中心，点 `Reset Mid`。该操作重设中心，不会主动锁住轴。
4. 设置 KP/KD，点 `Set KpKd`。当前网页默认 **KP=5、KD=10**；这是界面默认值，不是对所有改型的安全参数保证。
5. 在 `Change ID to`填目标编号，点 `Change ID`。
6. 重新扫描确认新 ID，贴上编号和位置标签。
7. 断电，换下一个舵机；15 个完成后再机械装配。

### ID 与关节

左右以**机器人自身**为准，装配方向以 PDF 第 1 页为准，不是面对机器人时观察者的左右。

| ID | 安装位置/关节 | ID | 安装位置/关节 |
|---|---|---|---|
| 10 | 左髋偏航 | 20 | 右髋偏航 |
| 11 | 左髋侧倾 | 21 | 右髋侧倾 |
| 12 | 左髋俯仰 | 22 | 右髋俯仰 |
| 13 | 左膝 | 23 | 右膝 |
| 14 | 左踝 | 24 | 右踝 |
| 30 | 颈部俯仰 | 31 | 头部俯仰 |
| 32 | 头部偏航 | 33 | 头部侧倾 |
| 34 | 嘴部，独立控制 |  |  |

左右安装有镜像差异，不能仅凭“同一类关节”复用朝向。ID34 不属于 14 维策略输出，但属于需要扫描和校准的 15 个舵机。

来源：[官网设置说明](https://www.xgorobot.com/duck.html#software-usage)、[设置页面代码](https://github.com/LuwuDynamics/xgoduck_runtime_arduino/blob/8837d5a3bc7cf3746f6da2b55c7b9191026b9838/assets/servo.js)、[策略关节顺序](https://github.com/LuwuDynamics/xgoduck_runtime_arduino/blob/8837d5a3bc7cf3746f6da2b55c7b9191026b9838/python/rl_core.py)。

## 第 6 步：按官方图装底座和双腿

**装配保持断电。** 副舵盘与输出轴不是一回事；按图拆副舵盘，不能拆输出轴。紧固以牢固且不破坏打印件为原则，官方没有可靠扭矩数值。

下表螺丝数量为**该页 PDF 原注**。部分页只示范单腿，未说明是否包含另一腿，不能自动翻倍或当作整机总数。

| PDF 页 | 操作 | 螺丝/要点 |
|---|---|---|
| 1 | 对照编号图检查 15 个舵机标签 | 10–14、20–24、30–34 |
| 2 | 处理副舵盘 | 保留30/31；10/20/32可选拆；其余按图拆，不拆输出轴 |
| 3 | ID20、10 固定到底部连接板 | M2×6，6颗；线穿中间开口，红圈两孔暂不装螺丝 |
| 4 | 固定底座支架 | M2×6，4颗 |
| 5 | 两侧舵盘装轴承和侧支架 | M2×6，8颗 |
| 6 | 固定腿部舵机，图示10/11一侧 | 先接线；M2×6，4颗 |
| 7 | 固定两侧腿部连接板 | M2×6，8颗 |
| 8 | 分别组合22/23、12/13 | 左右朝向不同，按图；M2×6，8颗 |
| 9 | 装腿部连杆和轴承，图示13一侧 | M2×6，4颗 |
| 10 | 安装脚部舵机与连杆 | 先接13/14线缆；M2×6，2颗 |
| 11 | 安装脚掌、背面轴承及轴件 | M2×6，4颗；另标M2.3×8但无数量；轴承10×15×3mm |
| 12 | 装腿部上端轴承和支架 | M2×6，4颗 |
| 13 | 将腿装到底座 | 先接线并按图放轴承；M2×6，4颗 |
| 14 | 固定腿部侧板 | M2.3×4，2颗，注明副舵盘自带螺丝 |
| 15 | 安装两块 TPU 脚垫，复核两腿ID | 方向按图；脚垫固定方式未完整说明 |

每封闭一个关节前确认：线已接好、未压在接缝里、有活动余量；左右零件和朝向匹配。若图中某处无法确定，不猜测插线或螺丝规格，先向供应商核实。

## 第 7 步：装头部、颈部、电池和控制板

| PDF 页 | 操作 | 螺丝/要点 |
|---|---|---|
| 16 | ID32 装轴承和支架 | 先接线；M2×6，4颗 |
| 17 | ID33 装支架及两侧轴承 | 先接底部线缆；M2×6，4颗 |
| 18 | 连接32/33组件 | 线缆按图绕行；M2×6，4颗 |
| 19 | 安装 ID34 嘴部舵机和外壳 | 完成走线；M2×6，4颗 |
| 20 | 固定内部支架及外壳 | M2×6，左图4颗、右图3颗 |
| 21 | 安装嘴部软垫 | 在箭头位置用双面胶固定，型号/厚度未指定 |
| 22 | 安装30/31与连接片，两面均装 | M2×6，共16颗 |
| 23 | 安装电池支架与电路板组件 | M2×6，4颗；M3×16，4颗 |
| 24 | 将上部支架装到下部组件 | M2×6，4颗 |
| 25 | 连接上下组件，两面分别固定 | M2×6，每面4颗，共8颗 |

电池先机械固定，电气连接留到接线、极性和短路检查完成后。线缆不能受拉、被头颈或腿部夹住，也不能妨碍预期活动范围。

**关于螺丝总数：** PDF 的 M2×6 页注按字面累计111颗，但多个单腿示范页的计数范围不清楚。因此111不是验证过的整机消耗量，也不能据此把采购200颗直接认定为多余89颗。

## 第 8 步：整机上电前验收

- [ ] 两腿和头颈舵机编号、安装朝向与 PDF 一致。
- [ ] 输出轴未误拆，副舵盘按第2页处理。
- [ ] 轴承、轴件及螺丝匹配，无明显松动、打印件开裂或强压变形。
- [ ] 线缆未被外壳压住，关节活动有余量。
- [ ] PCB修订、舵机接口、电池正负和充电器兼容性已确认。
- [ ] 机器人已可靠支撑，手指和工具离开运动夹点。
- [ ] 独立供电正确，具备可安全切断的执行器供电方式。
- [ ] App启动后，`Servos online`为 **15/15**，`IMU`为 **online**，无持续反馈错误。

任何一项不通过，先排查再开运动模式。当前运行代码不强制15/15才允许运动，**按钮能点不是验收通过**。

## 第 9 步：逐关节机械零位校准

**必须全程支撑。零位姿态不是站姿，零位下不能自行站立是正常的。**

先打开 [官网零位正视图](https://www.xgorobot.com/images/duck/software-usage/zero-pose.png)和 [侧视图](https://www.xgorobot.com/images/duck/software-usage/zero-pose-side.png)。连杆应按图呈垂直或水平；如果无法确定某个关节的机械零位，不继续猜测。

1. 进入 `Calibration`。当前程序会卸载全部舵机，**不会自动把机器人摆到零姿态**。
2. 手动摆好一个关节的机械零位。只在关节卸载时调整，不与已加载舵机对抗。
3. 点击对应 ID 的 `Set center & hold`。
4. 等待该关节显示 `Holding 2048`。该关节会上扭矩并保持，之前完成的关节继续保持，未完成的仍卸载。
5. 逐个完成其余关节，包括嘴部 ID34。顺序可按页面 ID 从10到34，先确认结构没有相互干涉。
6. 需要重新调整时点 `Unload all`释放已保持关节，再重做受影响的关节。卸载前确保整机有支撑。
7. 全部完成后点 `Exit`；退出会全部卸载，继续托住机器人，再回 `Control`。
8. 建议完成一次安全断电/重启复核，确认校准后的行为仍一致，记录校准日期和异常。

当前实现没有单独 `Save`按钮，也不使用旧指南中的 `data/zero_pos.json`。设零命令直接发送到舵机；本次未实机验证跨断电保存效果。

正常角度换算中心使用2047，校准保持显示2048是当前代码行为，**不要为了消除这一计数差自行修改校准**。校准保持增益KP3/KD0也不同于 Servo setup 的存储增益。

来源：[校准页面](https://github.com/LuwuDynamics/xgoduck_runtime_arduino/blob/8837d5a3bc7cf3746f6da2b55c7b9191026b9838/assets/calibrate.js)、[MCU固件](https://github.com/LuwuDynamics/xgoduck_runtime_arduino/blob/8837d5a3bc7cf3746f6da2b55c7b9191026b9838/sketch/sketch.ino)。

## 第 10 步：默认姿态与首次试走

### 支撑下测试

1. 启动App；当前初始模式为 `Inference only`，舵机卸载。
2. 重新确认15/15、IMU在线、反馈正常，并确保运动空间无夹点。
3. 在可靠支撑下点 `Default pose`，观察各关节方向和干涉。它会转向模型默认姿态，不是保持手动摆出的姿态。
4. 如出现反向、卡死、持续抖动、异常发热或线缆绷紧，立即停止并排查ID、朝向、校准和接线，不反复硬试。
5. 托住机器人，测试 `Torque off`能够卸载。之后再恢复默认姿态。

### 地面小幅试走

1. 确认独立电池供电已正确接通，再按官网流程拔USB；不要拔掉唯一供电后误判故障。
2. 将机器人放到平整、防滑、无桌边坠落风险的地面，清空周围障碍。
3. 普通无轮版保持 `Roller mode = Off`。
4. 点 `Walk / get up`，先做小幅、短时间摇杆指令。
5. 松摇杆观察是否停止移动，再测试 `Zero motion`。需要卸载时使用 `Torque off`。
6. 验证基础站姿和行走后再考虑叼取等技能；首次不主动推倒、不试翻滚、不换训练模型。

### 停止方式

| 操作 | 实际效果 |
|---|---|
| 松摇杆、`Zero motion` | 清零移动指令，不卸载扭矩 |
| 控制页失焦、转后台、离页 | 清零移动指令，保持模式和扭矩；校准页行为另有卸载保护 |
| 控制页 `Torque off` | 卸载舵机，机器人可能倒下 |
| 控制页按空格 | 当前代码等效卸载；须确保按键被该页面接收，不能视为硬件急停 |
| 网页/主机/总线无响应 | 在人身安全前提下切断执行器供电 |

软件看门狗不能替代经过验证的硬件急停。不要把关闭网页或Wi-Fi断开当成已经解除扭矩。

来源：[控制页](https://github.com/LuwuDynamics/xgoduck_runtime_arduino/blob/8837d5a3bc7cf3746f6da2b55c7b9191026b9838/assets/app.js)、[控制器](https://github.com/LuwuDynamics/xgoduck_runtime_arduino/blob/8837d5a3bc7cf3746f6da2b55c7b9191026b9838/python/controller.py)。

## 常见问题

| 现象 | 优先检查 |
|---|---|
| 找不到板卡或无法烧录 | USB是否数据线、是否只接一块板、EDL是否按原厂图、驱动和权限；不要带电改跳线 |
| 刷完仍不正常启动 | 是否成功写入、是否断电后移除EDL跳线、是否等待完整启动 |
| 舵机扫描缺失 | 独立供电、线束、PCB新旧映射、重复ID；单个排查前断电 |
| 进入校准后机器人软倒 | 当前程序首先卸载，这是预期行为；必须支撑，不靠上扭矩强撑 |
| 校准页没有Save | 当前版本没有该按钮，不按旧文档寻找zero_pos.json |
| 默认姿态扭曲或方向错误 | ID与左右朝向、机械装配、零位、模型/运行版本；先停止 |
| 关网页后关节仍硬 | 控制页只是停止移动，使用Torque off卸载 |
| 舵机持续嗡鸣、卡住或发热 | 立即停止，排查机械干涉、螺丝长度、轴承配合、零位和供电 |
| 第11/14页螺丝与文字指南不符 | 采用当前PDF标注；不确定的随舵机紧固件向供应商确认 |
| 旧打印清单找不到hip_yaw2rol | 当前名为2x_hip_yaw2rol.STL，按2件规划 |

## 最终验收与记录

- [ ] 15个唯一ID均在线，IMU在线，系统无持续错误。
- [ ] 校准后默认姿态正常，无反向、干涉、松动或异常温升。
- [ ] Torque off可用，卸载时有支撑，供电可安全切断。
- [ ] 地面小幅移动及清零移动正常。
- [ ] 硬件修订、安装软件/系统镜像、运行提交、舵机型号、模型版本已记录。
- [ ] 已保存材料/打印参数、装配照片、接线核对结果、校准日期和异常记录。

| 记录项 | 填写 |
|---|---|
| PCB版本/实际接口映射 |  |
| Uno Q型号、系统镜像 |  |
| 运行端提交/安装工具版本 |  |
| 舵机型号、固件及ID核对 |  |
| 电池与充电器型号/极性核对 |  |
| 打印机、材料、切片参数 |  |
| 校准日期与重启复核 |  |
| 默认姿态与试走结果 |  |
| 问题、照片与修复措施 |  |

## 文档冲突与核查边界

- 旧 `BUILD_GUIDE.md`将第11/14页螺丝写为M2.5×6，与当前PDF的M2.3×8/M2.3×4不同；将M3×16位置概括为22–23页，当前PDF实际在23页标注。
- 旧说明称进入校准会到2047，并要求备份zero_pos.json；当前界面是卸载后逐个设零保持，不使用该文件。
- 旧说明KP5/KD20与当前设置页默认KP5/KD10不同。
- 当前PDF第21页明确用双面胶；旧文字仍称胶种未说明。双面胶厚度与脚垫固定方法仍不完整。
- PDF没有完整轮滑装配步骤，没有每页STL名称对应，也没有统一说明单腿页螺丝数量是否含另一腿。
- 未提供可验证的完整打印参数、紧固扭矩、所有电源极性图和整机负载验证。本教程不填补这些数据。
- 本次已读取25页图文并静态核查软件流程，但没有实际烧录、装配、充电或动作测试。
- [硬件授权说明](https://github.com/LuwuDynamics/xgoduck_hardware/blob/b5e9ff16a98f7bbe5c146aad29ad38f0dde30a89/LICENSING.md)仍有待定条款；个人复刻与设计再分发应确认适用权限，商业使用另行核对。

## 官方资料

- [25页原版装配PDF](computer:///workspace/XGO-Duck-官方装配图.pdf)；[固定提交在线版本](https://github.com/LuwuDynamics/xgoduck_hardware/blob/b5e9ff16a98f7bbe5c146aad29ad38f0dde30a89/Assembly_Guide.pdf)。
- [硬件仓库](https://github.com/LuwuDynamics/xgoduck_hardware/tree/b5e9ff16a98f7bbe5c146aad29ad38f0dde30a89)、[BOM](https://github.com/LuwuDynamics/xgoduck_hardware/blob/b5e9ff16a98f7bbe5c146aad29ad38f0dde30a89/BOM.md)、[PCB安全公告](https://github.com/LuwuDynamics/xgoduck_hardware/blob/b5e9ff16a98f7bbe5c146aad29ad38f0dde30a89/PCBA/README.md)。
- [当前运行端README](https://github.com/LuwuDynamics/xgoduck_runtime_arduino/blob/8837d5a3bc7cf3746f6da2b55c7b9191026b9838/README.md)。
- [Duck官网安装/操作说明](https://www.xgorobot.com/duck.html#software-usage)、[零位正视图](https://www.xgorobot.com/images/duck/software-usage/zero-pose.png)、[零位侧视图](https://www.xgorobot.com/images/duck/software-usage/zero-pose-side.png)。
- [Arduino官方EDL操作](https://docs.arduino.cc/software/app-lab/configure/flash/#step-1-set-your-board-to-edl-mode)、[Duck安装软件清单](https://downloads.xgorobot.com/xgoduck/releases.json)、[系统镜像目录](https://luwu-file.oss-cn-beijing.aliyuncs.com/xgoduck/catalog.json)。
