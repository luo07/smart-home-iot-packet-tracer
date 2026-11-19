# 🏠 Smart Home IoT Automation & Security System | 智能家居物联网自动化与安防系统

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco%20Packet%20Tracer-IoT%20Simulation-1BA0D7?style=flat-square&logo=cisco)
![Architecture](https://img.shields.io/badge/Architecture-Centralized%20Edge%20IoT-success?style=flat-square)
![Security](https://img.shields.io/badge/Security-WPA2--PSK%20(AES)-blue?style=flat-square)
![Subnet](https://img.shields.io/badge/Subnet-192.168.25.0%2F24%20(DHCP)-orange?style=flat-square)

---

## 📖 Project Overview | 项目概述

This project designs, configures, and validates an offline-capable **Smart Home IoT Automation & Security System** in **Cisco Packet Tracer**. Built on a centralized edge computing architecture, the system integrates **climate synergy, automated lighting, RFID access control, fire/smoke exhaust, and privacy-preserving elderly fall detection** over a secured wireless local area network.

本项目基于 **Cisco Packet Tracer** 设计、配置并验证了一套支持离线运行的**智能家居物联网自动化与安防系统**。系统采用集中式边缘计算架构，在受加密保护的无线局域网内，集成了**温控协同、自动照明、RFID 门禁、火灾排烟以及隐私保护型老人跌倒检测**五大核心模块。

### 🎯 Key Performance Indicators (KPIs) | 核心性能指标
* **Energy Efficiency | 节能效率:** Targets a **20–30% daily energy waste reduction** by maintaining indoor temperature within `18.0°C – 26.0°C` via closed-loop AC, Ceiling Fan, and Furnace automation, alongside motion-activated lighting.
  * 通过空调、吊扇与暖气的闭环温控（维持室温在 `18.0°C – 26.0°C`）及人体感应照明，目标减少 **20%–30% 的日常能源浪费**。
* **Sub-2-Second Emergency Response | 2 秒内应急响应:** Achieves `≤ 2 seconds` automated response latency for critical hazards (e.g., automatic dual-window exhaust upon smoke detection).
  * 针对烟雾等紧急险情，实现 `≤ 2 秒` 的本地自动化响应（如自动开启双窗排烟）。
* **Strict Access Control | 严格门禁控制:** Enforces **100% authorized entry** by unlocking the Smart Door exclusively for valid RFID credentials (`Card ID = 1001`).
  * 仅对授权 RFID 卡（`Card ID = 1001`）自动解锁智能门，实现 **100% 授权准入控制**。
* **Privacy-Preserving Safety | 隐私保护型安全监测:** Detects bathroom fall incidents using a non-optical sudden-motion sensor (`Arefd Guard`) paired with an emergency siren (`Fall Alarm`), avoiding intrusive cameras in private zones.
  * 在卫生间等隐私区域采用非光学突发运动传感器（`Arefd Guard`）联动警报器（`Fall Alarm`），在保护住户隐私的前提下实现跌倒监测。

---

## 🗺️ Network Topology & Spatial Layout | 网络拓扑与空间布局

![Smart Home IoT Topology](images/topology.png)

The simulated residence covers **6 functional zones**: **Server Room**, **Master Bedroom**, **Kitchen**, **Work Room**, **Toilet**, and **Living Room**, plus an exterior environmental trigger (`Old Car`) for heat and smoke testing.

模拟住宅涵盖 **6 个功能区域**：**服务器机房 (Server Room)**、**主卧 (Master Bedroom)**、**厨房 (Kitchen)**、**书房 (Work Room)**、**卫生间 (Toilet)** 及 **客厅 (Living Room)**，并在室外配置了环境模拟触发源（`Old Car`）用于高温和烟雾测试。

---

## 🌐 Network Architecture & IP Addressing | 网络架构与 IP 规划

### 1. IP Addressing Scheme (`192.168.25.0/24`) | IP 地址分配方案
The network adopts a Class C subnet with static addressing for core infrastructure and automated **DHCP** for Plug-and-Play (PnP) IoT endpoint onboarding.
网络采用 C 类子网，核心基础设施使用静态 IP 保证稳定性，所有物联网终端与客户端采用 **DHCP** 实现“即插即用”。

| Device Category (设备类别) | Interface (接口) | SSID / Media (介质) | Security / PSK (加密与密码) | IP Address / DHCP Range (IP/范围) | Subnet Mask (子网掩码) | Default Gateway (默认网关) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Home Gateway (`DLC100`)** | Wireless / LAN | Internal (`HomeGateway`) | `WPA2-PSK` / `12345678` (AES) | `192.168.25.1` (Static) | `255.255.255.0` | Self |
| **Internal Server (`Server-PT`)** | `FastEthernet0` | Wired (有线) | N/A | `192.168.25.2` (Static) | `255.255.255.0` | `192.168.25.1` |
| **IoT Devices & Clients** | `Wireless0` (`PT-IOT-NM-1W`) | `HomeGateway` | `WPA2-PSK` / `12345678` (AES) | `192.168.25.3 – 192.168.25.254` (DHCP) | `255.255.255.0` | `192.168.25.1` |

### 2. End-to-End Data Communication Flow | 端到端数据通信流
1. **Data Generation (数据采集):** Environmental sensors (`TM`, `SD`, `Card Reader`) detect state changes and encapsulate telemetry into PDUs. / 传感器检测环境变化并将遥测数据封装为 PDU。
2. **Wireless Transmission (无线传输):** Telemetry is transmitted over `2.4 GHz` Wi-Fi (`WPA2-PSK AES`) via each device's `PT-IOT-NM-1W` wireless adapter. / 通过 `PT-IOT-NM-1W` 无线模块经加密 Wi-Fi 发送至网关。
3. **Edge Processing (边缘处理):** The **Home Gateway** (`192.168.25.1`) evaluates incoming payloads locally against registered `IF-THEN` conditions. / 家庭网关在本地实时比对预设的 `IF-THEN` 自动化规则。
4. **Command Execution (指令下发):** Once triggered, the gateway dispatches wireless control commands directly to target actuators (`AC`, `Fan`, `Furnace`, `Window 1 & 2`, `Smart Door`, `Fall Alarm`). / 满足条件后，网关立即向对应执行器下发无线控制指令。
5. **Client Monitoring (客户端监控):** Users monitor real-time status via `HTTP/TCP` on the **Laptop** (Work Room) or **Tablet PC** (Living Room). / 用户通过书房电脑或客厅平板经由 `HTTP/TCP` 实时查看并管理所有设备。

---

## 📦 Hardware Inventory | 硬件设备清单 (15 个联网终端)

| Subsystem (子系统) | Device Label (设备名称) | Hardware Type (硬件类型) | Zone (区域) | Role (功能) |
| :--- | :--- | :--- | :--- | :--- |
| **Core & Clients**<br>核心与终端 | `Home Gateway` / `Server` | `DLC100` / `Server-PT` | Server Room | Central wireless AP, IoT controller & backend server / 无线网关、IoT 控制器与后台服务器 |
| | `Laptop` / `Tablet` | `Laptop-PT` / `TabletPC-PT` | Work Room / Living Room | Configuration terminal & daily monitoring UI / 配置终端与日常触控监控端 |
| **Climate Synergy**<br>温控协同 | `TM` | Temperature Monitor | Living Room | Real-time ambient temperature sensing / 实时室温监测 |
| | `AC` / `Fan` / `Furnace` | AC / Ceiling Fan / Furnace | Living Room | Synergistic cooling & automated heating / 空调风扇协同制冷与暖气自动制热 |
| **Access Control**<br>门禁安防 | `Card Reader` | RFID Reader | Front Entrance | Authenticates `Smart Card` (`ID: 1001`) vs. `Unknown Card` / 识别授权卡与未知卡 |
| | `Smart Door` | Smart Door | Front Entrance | Contactless automated locking/unlocking / 非接触式自动开锁与上锁 |
| **Fire & Exhaust**<br>消防排烟 | `SD` | Smoke Detector | Living Room | Monitors atmospheric smoke/CO concentration / 监测烟雾与一氧化碳浓度 |
| | `Window 1` & `Window 2` | Smart Windows | Living Room / Kitchen | Automated cross-ventilation for smoke exhaust / 自动开窗通风排烟 |
| **Lighting**<br>智能照明 | `MD` | Motion Detector | Living Room | Occupancy detection for lighting / 客厅人体存在感应 |
| | `Light 1` & `Light 2` | Smart Lights | Living Room / Bedroom | Automated occupancy-based illumination / 感应自动开关灯 |
| **Elderly Safety**<br>老人防跌倒 | `Arefd Guard` | Motion Detector | Toilet | Non-optical sudden-motion fall detection / 非光学突发动作跌倒检测 |
| | `Fall Alarm` | Siren | Toilet | Immediate audible/visual emergency alert / 紧急声光警报 |
| **Test Trigger**<br>测试源 | `Car` | Old Car | Exterior | Simulated heat and smoke emission source / 模拟高温与尾气烟雾源 |

---

## ⚙️ Automation Logic (11 Closed-Loop Rules) | 自动化闭环逻辑

![IoT Conditions](images/conditions.png)

All rules are configured in **IoT Monitor (`Device Conditions`)** using closed-loop logic to prevent actuator state lock-in.
所有规则均在网关 **IoT Monitor (`Conditions`)** 中采用闭环逻辑配置，防止执行器状态锁死：

| Rule Name (规则名) | Trigger Condition (触发条件) | Automated Action(s) (执行动作) |
| :--- | :--- | :--- |
| `AC_ON` | `TM Temperature >= 26.0 °C` | Turn ON `AC` & Set `Fan` to `High` (开空调 + 风扇高速) |
| `AC_OFF` | `TM Temperature < 24.0 °C` | Turn OFF `AC` & Set `Fan` to `Low` (关空调 + 风扇低速) |
| `Heater_Auto_On` | `TM Temperature < 18.0 °C` | Turn ON `Furnace` (开启暖气) |
| `Heater_Auto_OFF` | `TM Temperature > 21.0 °C` | Turn OFF `Furnace` (关闭暖气) |
| `Motion_Light_ON` | `MD On is true` | Turn ON `Light 1` & `Light 2` (开启客厅与卧室灯) |
| `Motion_Light_OFF` | `MD On is false` | Turn OFF `Light 1` & `Light 2` (关闭客厅与卧室灯) |
| `Smoke_Open_Window` | `SD Level > 1` | Open `Window 1` & `Window 2` (开启双窗排烟) |
| `Smoke_Off_Window` | `SD Level < 0.5` | Close `Window 1` & `Window 2` (烟雾散去后自动关窗) |
| `Open_Door` | `Card Reader Card ID = 1001` | Unlock `Smart Door` & Set Reader to `Valid` (验证通过并开锁) |
| `Close_Door` | `Card Reader Card ID != 1001` | Lock `Smart Door` & Set Reader to `Invalid` (拒绝访问并上锁) |
| `Toilet_Fall_Detector` | `Arefd Guard On is true` | Activate `Fall Alarm` Siren (触发卫生间跌倒警报) |

---

## 🧪 Verification & Test Results | 测试与验证结果

Validated via Packet Tracer's **Manual Interaction (`Alt + Click`)** and **Environment Manager (`Shift + E`)**:

| Test Scenario (测试场景) | Input / Simulated Value (输入/模拟值) | Verified Actual Outcome (实际验证结果) | Status |
| :--- | :--- | :--- | :---: |
| **Overheating (`AC_ON`)** | `TM = 33.60 °C` (`≥ 26.0 °C`) | `AC` turned ON; `Fan` switched to `High` / 空调开启，风扇高速 | ✅ Pass |
| **Optimal Temp (`AC_OFF`)** | `TM = 20.27 °C` (`< 24.0 °C`) | `AC` turned OFF; `Fan` stepped down to `Low` / 空调关闭，风扇低速 | ✅ Pass |
| **Cold Heating (`Heater_Auto_On`)** | `TM = 10.18 °C` (`< 18.0 °C`) | `Furnace` turned ON / 暖气自动开启 | ✅ Pass |
| **Warm Heating (`Heater_Auto_OFF`)** | `TM = 23.00 °C` (`> 21.0 °C`) | `Furnace` turned OFF / 暖气自动关闭 | ✅ Pass |
| **Smoke Hazard (`Smoke_Open_Window`)** | `SD Level > 1` | `SD` LED turned Red; `Window 1 & 2` opened / 探测器亮红灯，双窗开启 | ✅ Pass |
| **Air Cleared (`Smoke_Off_Window`)** | `SD Level = 0.000025` (`< 0.5`) | `Window 1 & 2` automatically closed / 双窗自动关闭 | ✅ Pass |
| **Authorized RFID (`Open_Door`)** | Drag `Smart Card` (`ID: 1001`) | Reader `Valid` (Green LED); `Smart Door` unlocked / 绿灯亮起，门解锁 | ✅ Pass |
| **Unauthorized RFID (`Close_Door`)** | Drag `Unknown Card` | Reader `Invalid` (Red LED); `Smart Door` locked / 红灯亮起，门保持锁定 | ✅ Pass |
| **Occupancy Lighting** | `MD` toggled (`True` / `False`) | `Light 1 & 2` switched ON / OFF accordingly / 灯光随感应自动开关 | ✅ Pass |
| **Bathroom Fall Alert** | `Alt + Hover` on `Arefd Guard` | `Fall Alarm` Siren activated immediately / 跌倒警报立即响起 | ✅ Pass |

---

## 🛠️ Engineering Challenges & Troubleshooting | 工程难点与排错记录

1. **IoT Devices Not Appearing in IoT Monitor | 物联网设备未在监控列表上线**
   * **Issue (原因):** Endpoints default to wired `FastEthernet0` adapters and do not auto-register with the wireless gateway. / 设备默认配备有线网卡，且未绑定无线网关。
   * **Resolution (解决):** Swapped each device's module to `PT-IOT-NM-1W` under **Advanced > I/O Config**, configured SSID `HomeGateway` (WPA2-PSK), and bound **Config > Settings > IoT Server** to `Home Gateway`. / 更换为无线模块并手动在 Settings 中将 IoT Server 绑定至 `Home Gateway`。
2. **Actuator State Retention | 执行器状态无法自动复位**
   * **Issue (原因):** Packet Tracer uses one-way state triggers; windows stayed open even after smoke cleared. / 规则引擎默认为单向触发，烟雾散去后窗户不会自动关闭。
   * **Resolution (解决):** Implemented closed-loop automation by adding `Smoke_Off_Window` (`SD Level < 0.5`) to reset windows to `false`. / 新增反向关闭规则，构建完整闭环控制。
3. **Automation Logic Conflicts | 自动化控制环路冲突**
   * **Issue (原因):** Assigning the Ceiling Fan to both cooling (`High`) and fire safety (`Low`/`Off`) caused control-loop conflicts when heat and smoke occurred simultaneously. / 吊扇同时受温控与消防规则控制，高温伴随烟雾时产生逻辑冲突。
   * **Resolution (解决):** Decoupled the Ceiling Fan from fire logic—isolating smoke exhaust exclusively to `Window 1 & 2` and climate circulation to `AC` + `Fan`. / 将吊扇与消防逻辑彻底解耦，排烟完全交由智能窗负责。

---

## 🚀 How to Run & Replicate | 运行与复现指南

### Quick Start | 快速运行
1. Download `Smart_Home_IoT.pkt` and open it in **Cisco Packet Tracer** (v8.0+). / 下载并在 Cisco Packet Tracer 中打开 `.pkt` 文件。
2. Click **Tablet** or **Laptop** → **Desktop** → **Web Browser**. / 打开平板或电脑的浏览器。
3. Navigate to `http://192.168.25.1` and log in (`Username: admin`, `Password: admin`). / 访问网关地址并登录。
4. Open **Home** to view live states or **Conditions** to inspect all 11 rules. / 查看设备实时状态或 11 条自动化规则。
5. **Test Triggers (测试触发):**
   * Drag `Smart Card` / `Unknown Card` over `Card Reader`. / 拖动卡片至读卡器测试门禁。
   * Hold `Alt` and hover over `MD` or `Arefd Guard`. / 按住 `Alt` 悬停于运动传感器测试照明与跌倒警报。
   * Press `Shift + E` (Environment Manager) or `Alt + Click` on `Old Car` to adjust temperature/smoke. / 使用环境面板或 `Alt + 点击` 汽车调节温度与烟雾浓度。

---

## 📂 Repository Structure | 仓库目录结构
```text
smart-home-iot-packet-tracer/
├── Smart_Home_IoT.pkt          # Cisco Packet Tracer simulation file | 模拟源文件
├── images/
│   ├── topology.png            # Connected smart home topology | 完整连线拓扑图
│   └── conditions.png          # IoT Monitor 11 automation rules | 11条自动化规则截图
└── README.md                   # Bilingual project documentation | 中英双语项目文档
