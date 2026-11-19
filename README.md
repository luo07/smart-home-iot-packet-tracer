# 🏠 Smart Home IoT Automation & Security System / 智能家居物联网自动化与安防系统

<div align="center">

**An offline-capable Smart Home IoT automation and security system simulated in Cisco Packet Tracer**  
**基于 Cisco Packet Tracer 构建的集中式边缘智能家居物联网自动化与安防系统**

<br>

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco%20Packet%20Tracer-IoT%20Simulation-1BA0D7?style=flat-square&logo=cisco)
![Architecture](https://img.shields.io/badge/Architecture-Centralized%20Edge%20IoT-success?style=flat-square)
![Security](https://img.shields.io/badge/Security-WPA2--PSK%20(AES)-blue?style=flat-square)
![Subnet](https://img.shields.io/badge/Subnet-192.168.25.0%2F24%20(DHCP)-orange?style=flat-square)

<br>

[English Version](#english-version) • [中文版本](#中文版本)

</div>

---

# English Version

## 📖 Project Overview
This project designs, configures, and validates an offline-capable **Smart Home IoT Automation & Security System** in **Cisco Packet Tracer**. Built on a centralized edge computing architecture, the system integrates **climate synergy, automated lighting, RFID access control, fire/smoke exhaust, and privacy-preserving elderly fall detection** over a secured wireless local area network.

### 🎯 Key Performance Indicators (KPIs)
* **Energy Efficiency:** Targets a **20–30% daily energy waste reduction** by maintaining indoor temperature within `18.0°C – 26.0°C` via closed-loop AC, Ceiling Fan, and Furnace automation, alongside motion-activated lighting.
* **Sub-2-Second Emergency Response:** Achieves `≤ 2 seconds` automated response latency for critical hazards (e.g., automatic dual-window exhaust upon smoke detection).
* **Strict Access Control:** Enforces **100% authorized entry** by unlocking the Smart Door exclusively for valid RFID credentials (`Card ID = 1001`).
* **Privacy-Preserving Safety:** Detects bathroom fall incidents using a non-optical sudden-motion sensor (`Arefd Guard`) paired with an emergency siren (`Fall Alarm`), avoiding intrusive cameras in private zones.

---

## 🗺️ Network Topology & Spatial Layout
![Smart Home IoT Topology](images/topology.png)

The simulated residence covers **6 functional zones**: **Server Room**, **Master Bedroom**, **Kitchen**, **Work Room**, **Toilet**, and **Living Room**, plus an exterior environmental trigger (`Old Car`) for heat and smoke testing.

---

## 🌐 Network Architecture & IP Addressing

### 1. IP Addressing Scheme (`192.168.25.0/24`)
The network adopts a Class C subnet with static addressing for core infrastructure and automated **DHCP** for Plug-and-Play (PnP) IoT endpoint onboarding.

| Device Category | Interface | SSID / Media | Security / PSK | IP Address / DHCP Range | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Home Gateway (`DLC100`)** | Wireless / LAN | Internal (`HomeGateway`) | `WPA2-PSK` / `12345678` (AES) | `192.168.25.1` (Static) | `255.255.255.0` | Self |
| **Internal Server (`Server-PT`)** | `FastEthernet0` | Wired (Ethernet) | N/A | `192.168.25.2` (Static) | `255.255.255.0` | `192.168.25.1` |
| **IoT Devices & Clients** | `Wireless0` (`PT-IOT-NM-1W`) | `HomeGateway` | `WPA2-PSK` / `12345678` (AES) | `192.168.25.3 – 192.168.25.254` (DHCP) | `255.255.255.0` | `192.168.25.1` |

### 2. End-to-End Data Communication Flow
1. **Data Generation (Input):** Environmental sensors (`TM`, `SD`, `Card Reader`) detect state changes and encapsulate telemetry into PDUs.
2. **Wireless Transmission:** Telemetry is transmitted over `2.4 GHz` Wi-Fi (`WPA2-PSK AES`) via each device's `PT-IOT-NM-1W` wireless adapter.
3. **Edge Processing:** The **Home Gateway** (`192.168.25.1`) evaluates incoming payloads locally against registered `IF-THEN` automation conditions.
4. **Command Execution (Output):** Once triggered, the gateway dispatches wireless control commands directly to target actuators (`AC`, `Fan`, `Furnace`, `Window 1 & 2`, `Smart Door`, `Fall Alarm`).
5. **Client Monitoring (`HTTP/TCP`):** Residents and administrators monitor real-time status via the **IoT Monitor** web dashboard on the **Laptop** (Work Room) or **Tablet PC** (Living Room).

---

## 📦 Hardware Inventory (15 Connected IoT Endpoints)

| Subsystem | Device Label | Hardware Type | Zone | Role |
| :--- | :--- | :--- | :--- | :--- |
| **Core & Clients** | `Home Gateway` / `Server` | `DLC100` / `Server-PT` | Server Room | Central wireless AP, IoT controller, and backend server |
| | `Laptop` / `Tablet` | `Laptop-PT` / `TabletPC-PT` | Work Room / Living Room | Configuration terminal & portable daily monitoring UI |
| **Climate Synergy** | `TM` | Temperature Monitor | Living Room | Real-time ambient temperature sensing |
| | `AC` / `Fan` / `Furnace` | Air Conditioner / Ceiling Fan / Furnace | Living Room | Synergistic cooling (AC + Fan circulation) & automated heating |
| **Access Control** | `Card Reader` | RFID Reader | Front Entrance | Authenticates `Smart Card` (`ID: 1001`) vs. `Unknown Card` |
| | `Smart Door` | Smart Door | Front Entrance | Contactless automated locking/unlocking |
| **Fire & Exhaust** | `SD` | Smoke Detector | Living Room | Monitors atmospheric smoke/CO concentration |
| | `Window 1` & `Window 2` | Smart Windows | Living Room / Kitchen | Automated cross-ventilation for rapid smoke exhaustion |
| **Lighting** | `MD` | Motion Detector | Living Room | Occupancy detection for energy-saving lighting |
| | `Light 1` & `Light 2` | Smart Lights | Living Room / Bedroom | Automated occupancy-based illumination |
| **Elderly Safety** | `Arefd Guard` | Motion Detector | Toilet | Non-optical sudden-motion fall detection |
| | `Fall Alarm` | Siren | Toilet | Immediate audible/visual emergency alert |
| **Test Trigger** | `Car` | Old Car | Exterior | Simulated heat and smoke emission source |

---

## ⚙️ Automation Logic (11 Closed-Loop IoT Rules)
![IoT Conditions](images/conditions.png)

| Rule Name | Trigger Condition | Automated Action(s) |
| :--- | :--- | :--- |
| `AC_ON` | `TM Temperature >= 26.0 °C` | Set `AC` On to `true` & Set `Fan` Status to `High` |
| `AC_OFF` | `TM Temperature < 24.0 °C` | Set `AC` On to `false` & Set `Fan` Status to `Low` |
| `Heater_Auto_On` | `TM Temperature < 18.0 °C` | Set `Furnace` On to `true` |
| `Heater_Auto_OFF` | `TM Temperature > 21.0 °C` | Set `Furnace` On to `false` |
| `Motion_Light_ON` | `MD On is true` | Set `Light 1` Status to `On` & Set `Light 2` Status to `On` |
| `Motion_Light_OFF` | `MD On is false` | Set `Light 1` Status to `Off` & Set `Light 2` Status to `Off` |
| `Smoke_Open_Window` | `SD Level > 1` | Set `Window 1` On to `true` & Set `Window 2` On to `true` |
| `Smoke_Off_Window` | `SD Level < 0.5` | Set `Window 1` On to `false` & Set `Window 2` On to `false` |
| `Open_Door` | `Card Reader Card ID = 1001` | Set `Smart Door` Lock to `Unlock` & `Card Reader` Status to `Valid` |
| `Close_Door` | `Card Reader Card ID != 1001` | Set `Smart Door` Lock to `Lock` & `Card Reader` Status to `Invalid` |
| `Toilet_Fall_Detector` | `Arefd Guard On is true` | Set `Fall Alarm` On to `true` |

---

## 🧪 Verification & Test Results

| Test Scenario | Input / Simulated Value | Verified Actual Outcome | Status |
| :--- | :--- | :--- | :---: |
| **Overheating Cooling (`AC_ON`)** | `TM = 33.60 °C` (`≥ 26.0 °C`) | `AC` turned ON; `Fan` switched to `High` | ✅ Pass |
| **Optimal Cooling (`AC_OFF`)** | `TM = 20.27 °C` (`< 24.0 °C`) | `AC` turned OFF; `Fan` stepped down to `Low` | ✅ Pass |
| **Cold Heating (`Heater_Auto_On`)** | `TM = 10.18 °C` (`< 18.0 °C`) | `Furnace` turned ON | ✅ Pass |
| **Warm Heating (`Heater_Auto_OFF`)** | `TM = 23.00 °C` (`> 21.0 °C`) | `Furnace` turned OFF | ✅ Pass |
| **Smoke Hazard (`Smoke_Open_Window`)** | `SD Level > 1` | `SD` LED turned Red; `Window 1` & `Window 2` opened | ✅ Pass |
| **Air Cleared (`Smoke_Off_Window`)** | `SD Level = 0.000025` (`< 0.5`) | `Window 1` & `Window 2` automatically closed | ✅ Pass |
| **Authorized RFID (`Open_Door`)** | Drag `Smart Card` (`ID: 1001`) | Reader Status `Valid` (Green LED); `Smart Door` unlocked | ✅ Pass |
| **Unauthorized RFID (`Close_Door`)** | Drag `Unknown Card` | Reader Status `Invalid` (Red LED); `Smart Door` locked | ✅ Pass |
| **Occupancy Lighting** | `MD` state toggled (`True` / `False`) | `Light 1` & `Light 2` switched ON / OFF accordingly | ✅ Pass |
| **Bathroom Fall Alert** | `Alt + Hover` on `Arefd Guard` | `Fall Alarm` Siren activated immediately | ✅ Pass |

---

## 🛠️ Engineering Challenges & Troubleshooting
1. **IoT Devices Not Appearing in IoT Monitor:** Endpoints default to wired `FastEthernet0` adapters. Resolved by swapping to `PT-IOT-NM-1W` under **Advanced > I/O Config**, configuring SSID `HomeGateway` (WPA2-PSK), and binding **Config > Settings > IoT Server** to `Home Gateway`.
2. **Actuator State Retention:** Packet Tracer uses one-way state triggers, leaving windows open after smoke cleared. Resolved by implementing closed-loop automation with a dedicated `Smoke_Off_Window` (`SD Level < 0.5`) condition.
3. **Automation Logic Conflicts:** Assigning the Ceiling Fan to both cooling (`High`) and fire safety (`Low`/`Off`) caused control-loop oscillation. Resolved by decoupling the fan from fire logic—isolating smoke exhaust exclusively to `Window 1 & 2`.

---

## 🚀 How to Run & Replicate
1. Download `Smart_Home_IoT.pkt` and open it in **Cisco Packet Tracer** (v8.0+).
2. Click **Tablet** or **Laptop** → **Desktop** → **Web Browser**.
3. Navigate to `http://192.168.25.1` and log in (`Username: admin`, `Password: admin`).
4. Inspect live states under **Home** and all 11 rules under **Conditions**.
5. **Trigger Tests:** Drag `Smart Card` / `Unknown Card` over `Card Reader`; hold `Alt` and hover over `MD` or `Arefd Guard`; or press `Shift + E` (Environment Manager) / `Alt + Click` on `Old Car` to adjust temperature and smoke levels.

---

<br>

# 中文版本

[⬆️ 返回顶部 / Back to Top](#-smart-home-iot-automation--security-system--智能家居物联网自动化与安防系统)

## 📖 项目概述
本项目基于 **Cisco Packet Tracer** 设计、配置并验证了一套支持离线运行的**智能家居物联网自动化与安防系统**。系统采用集中式边缘计算架构，在受加密保护的无线局域网内，集成了**温控协同、自动照明、RFID 门禁、火灾排烟以及隐私保护型老人跌倒检测**五大核心模块。

### 🎯 核心性能指标 (KPIs)
* **节能效率：** 通过空调、吊扇与暖气的闭环温控（将室温维持在 `18.0°C – 26.0°C`）以及人体感应自动照明，目标减少 **20%–30% 的日常能源浪费**。
* **2 秒内应急响应：** 针对烟雾等紧急险情，实现 `≤ 2 秒` 的本地边缘自动化响应（如自动开启双窗排烟）。
* **严格门禁控制：** 仅对授权 RFID 卡（`Card ID = 1001`）自动解锁智能门，实现 **100% 授权准入控制**。
* **隐私保护型安全监测：** 在卫生间等隐私区域采用非光学突发运动传感器（`Arefd Guard`）联动警报器（`Fall Alarm`），避免使用摄像头，在保护住户隐私的同时精准监测跌倒风险。

---

## 🗺️ 网络拓扑与空间布局
![Smart Home IoT Topology](images/topology.png)

模拟住宅涵盖 **6 个功能区域**：**服务器机房 (Server Room)**、**主卧 (Master Bedroom)**、**厨房 (Kitchen)**、**书房 (Work Room)**、**卫生间 (Toilet)** 及 **客厅 (Living Room)**，并在室外配置了环境模拟触发源（`Old Car`）用于高温和烟雾测试。

---

## 🌐 网络架构与 IP 规划

### 1. IP 地址分配方案 (`192.168.25.0/24`)
网络采用 C 类子网，核心基础设施使用静态 IP 保证稳定性，所有物联网终端与客户端采用 **DHCP** 实现“即插即用 (Plug-and-Play)”。

| 设备类别 | 接口 | SSID / 传输介质 | 安全协议 / 密码 | IP 地址 / DHCP 范围 | 子网掩码 | 默认网关 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **家庭网关 (`DLC100`)** | Wireless / LAN | Internal (`HomeGateway`) | `WPA2-PSK` / `12345678` (AES) | `192.168.25.1` (静态) | `255.255.255.0` | Self |
| **内部服务器 (`Server-PT`)** | `FastEthernet0` | 有线以太网 | N/A | `192.168.25.2` (静态) | `255.255.255.0` | `192.168.25.1` |
| **物联网终端与客户端** | `Wireless0` (`PT-IOT-NM-1W`) | `HomeGateway` | `WPA2-PSK` / `12345678` (AES) | `192.168.25.3 – 192.168.25.254` (DHCP) | `255.255.255.0` | `192.168.25.1` |

### 2. 端到端数据通信流
1. **数据采集 (Input)：** 传感器（`TM`、`SD`、`Card Reader` 等）检测环境变化并将遥测数据封装为 PDU。
2. **无线传输：** 设备通过 `PT-IOT-NM-1W` 无线模块经 `2.4 GHz` 加密 Wi-Fi 将数据发送至网关。
3. **边缘处理：** **家庭网关**（`192.168.25.1`）在本地实时比对预设的 `IF-THEN` 自动化规则。
4. **指令下发 (Output)：** 满足触发条件后，网关立即向对应执行器（空调、风扇、暖气、智能窗、智能门、警报器）下发无线控制指令。
5. **客户端监控 (`HTTP/TCP`)：** 用户可通过书房电脑（Laptop）或客厅平板（Tablet PC）访问 **IoT Monitor** 实时查看设备状态。

---

## 📦 硬件设备清单 (15 个联网终端)

| 子系统 | 设备名称 | 硬件类型 | 所在区域 | 功能说明 |
| :--- | :--- | :--- | :--- | :--- |
| **核心与终端** | `Home Gateway` / `Server` | `DLC100` / `Server-PT` | Server Room | 集中式无线接入点、IoT 控制器与后台服务器 |
| | `Laptop` / `Tablet` | `Laptop-PT` / `TabletPC-PT` | Work Room / Living Room | 网络配置终端与日常触控监控端 |
| **温控协同** | `TM` | Temperature Monitor | Living Room | 实时环境温度监测 |
| | `AC` / `Fan` / `Furnace` | 空调 / 吊扇 / 暖气 | Living Room | 空调与风扇协同制冷及低温自动制热 |
| **门禁安防** | `Card Reader` | RFID Reader | Front Entrance | 识别授权卡（`ID: 1001`）与未知卡 |
| | `Smart Door` | Smart Door | Front Entrance | 非接触式自动解锁与上锁 |
| **消防排烟** | `SD` | Smoke Detector | Living Room | 监测空气烟雾与一氧化碳浓度 |
| | `Window 1` & `Window 2` | Smart Windows | Living Room / Kitchen | 自动开窗实现空气对流排烟 |
| **智能照明** | `MD` | Motion Detector | Living Room | 客厅人体存在感应 |
| | `Light 1` & `Light 2` | Smart Lights | Living Room / Bedroom | 人体感应自动开关照明 |
| **老人防跌倒** | `Arefd Guard` | Motion Detector | Toilet | 非光学突发动作跌倒检测 |
| | `Fall Alarm` | Siren | Toilet | 紧急声光警报 |
| **测试触发源** | `Car` | Old Car | Exterior | 模拟高温与尾气烟雾排放源 |

---

## ⚙️ 自动化闭环逻辑 (11 条核心规则)
![IoT Conditions](images/conditions.png)

| 规则名称 | 触发条件 | 自动执行动作 |
| :--- | :--- | :--- |
| `AC_ON` | `TM Temperature >= 26.0 °C` | 开启 `AC`，并将 `Fan` 调至 `High`（高速） |
| `AC_OFF` | `TM Temperature < 24.0 °C` | 关闭 `AC`，并将 `Fan` 调至 `Low`（低速） |
| `Heater_Auto_On` | `TM Temperature < 18.0 °C` | 开启 `Furnace`（暖气） |
| `Heater_Auto_OFF` | `TM Temperature > 21.0 °C` | 关闭 `Furnace`（暖气） |
| `Motion_Light_ON` | `MD On is true` | 开启 `Light 1` 与 `Light 2` |
| `Motion_Light_OFF` | `MD On is false` | 关闭 `Light 1` 与 `Light 2` |
| `Smoke_Open_Window` | `SD Level > 1` | 开启 `Window 1` 与 `Window 2` 排烟 |
| `Smoke_Off_Window` | `SD Level < 0.5` | 关闭 `Window 1` 与 `Window 2` |
| `Open_Door` | `Card Reader Card ID = 1001` | 解锁 `Smart Door`，读卡器状态设为 `Valid` |
| `Close_Door` | `Card Reader Card ID != 1001` | 锁定 `Smart Door`，读卡器状态设为 `Invalid` |
| `Toilet_Fall_Detector` | `Arefd Guard On is true` | 触发 `Fall Alarm` 警报器 |

---

## 🧪 测试与验证结果

| 测试场景 | 输入 / 模拟数值 | 实际验证结果 | 状态 |
| :--- | :--- | :--- | :---: |
| **高温制冷 (`AC_ON`)** | `TM = 33.60 °C` (`≥ 26.0 °C`) | 空调自动开启，吊扇切换为高速 | ✅ 通过 |
| **恒温节能 (`AC_OFF`)** | `TM = 20.27 °C` (`< 24.0 °C`) | 空调自动关闭，吊扇降为低速 | ✅ 通过 |
| **低温制热 (`Heater_Auto_On`)** | `TM = 10.18 °C` (`< 18.0 °C`) | 暖气自动开启 | ✅ 通过 |
| **达温停机 (`Heater_Auto_OFF`)** | `TM = 23.00 °C` (`> 21.0 °C`) | 暖气自动关闭 | ✅ 通过 |
| **烟雾报警 (`Smoke_Open_Window`)** | `SD Level > 1` | 烟感红灯亮起，双窗自动打开 | ✅ 通过 |
| **空气恢复 (`Smoke_Off_Window`)** | `SD Level = 0.000025` (`< 0.5`) | 双窗自动关闭复位 | ✅ 通过 |
| **授权刷卡 (`Open_Door`)** | 拖动 `Smart Card` (`ID: 1001`) | 读卡器绿灯亮起，智能门解锁 | ✅ 通过 |
| **非法刷卡 (`Close_Door`)** | 拖动 `Unknown Card` | 读卡器红灯亮起，智能门保持锁定 | ✅ 通过 |
| **感应照明** | 触发/取消 `MD` 感应状态 | `Light 1` 与 `Light 2` 同步开关 | ✅ 通过 |
| **卫生间跌倒警报** | `Alt + 悬停` 于 `Arefd Guard` | `Fall Alarm` 警报立即触发 | ✅ 通过 |

---

## 🛠️ 工程难点与排错记录
1. **物联网设备未在监控列表上线：** 设备默认配备有线网卡 `FastEthernet0`。通过在 **Advanced > I/O Config** 更换为 `PT-IOT-NM-1W` 无线模块、配置 WPA2-PSK 无线参数，并在 **Config > Settings** 中将 `IoT Server` 绑定至 `Home Gateway` 解决。
2. **执行器状态无法自动复位：** Packet Tracer 规则引擎默认为单向触发，烟雾散去后窗户不会自动关闭。通过新增 `Smoke_Off_Window`（`SD Level < 0.5`）反向规则构建完整闭环控制。
3. **自动化控制环路冲突：** 吊扇同时受温控（高速）与消防（低速/关）规则控制，高温伴随烟雾时会产生震荡冲突。通过将吊扇与消防逻辑彻底解耦，排烟任务完全交由双智能窗负责。

---

## 🚀 运行与复现指南
1. 下载 `Smart_Home_IoT.pkt` 并使用 **Cisco Packet Tracer** (v8.0+) 打开。
2. 点击 **Tablet** 或 **Laptop** → **Desktop** → **Web Browser**。
3. 访问 `http://192.168.25.1` 并登录（账号：`admin`，密码：`admin`）。
4. 在 **Home** 查看设备实时状态，或在 **Conditions** 查看全部 11 条自动化规则。
5. **交互测试：** 拖动卡片至 `Card Reader` 测试门禁；按住 `Alt` 悬停于 `MD` / `Arefd Guard` 测试感应；按 `Shift + E` 打开环境面板或 `Alt + 点击` `Old Car` 模拟温度与烟雾变化。

---

## 📂 Repository Structure / 仓库目录结构
```text
smart-home-iot-packet-tracer/
├── Smart_Home_IoT.pkt          # Cisco Packet Tracer simulation file / 模拟源文件
├── images/
│   ├── topology.png            # Connected smart home topology / 完整连线拓扑图
│   └── conditions.png          # IoT Monitor 11 automation rules / 11条自动化规则截图
└── README.md                   # Bilingual project documentation / 中英双语项目文档
