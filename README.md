# Smart Home IoT Automation & Security System (Cisco Packet Tracer)

<p align="center">
  <a href="#english-version">English Version</a> •
  <a href="#中文版本">中文版本</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Simulation-Cisco_Packet_Tracer-1BA0D7?style=flat-square" alt="Cisco Packet Tracer" />
  <img src="https://img.shields.io/badge/Architecture-Centralized_Edge_IoT-2E7D32?style=flat-square" alt="Centralized Edge IoT" />
  <img src="https://img.shields.io/badge/Security-WPA2--PSK_AES-F57C00?style=flat-square" alt="WPA2-PSK AES" />
  <img src="https://img.shields.io/badge/Subnet-192.168.25.0%2F24_DHCP-1565C0?style=flat-square" alt="Subnet 192.168.25.0/24" />
</p>

---

<a id="english-version"></a>
## English Version

### 1. Project Overview
A centralized edge-computing **Smart Home IoT Automation & Security System** built and verified in **Cisco Packet Tracer**. The system connects **15 IoT endpoints** across **6 residential zones** through a Home Gateway (`DLC100`) and internal server (`192.168.25.0/24`), automating climate control, motion-activated lighting, RFID door access, smoke emergency venting, and privacy-preserving non-optical elderly fall detection.

### 2. Key Performance Indicators (KPIs)
| Metric | Target / Verified Outcome |
| :--- | :--- |
| **Energy Efficiency** | 20–30% daily HVAC & lighting waste reduction; indoor temperature maintained at `18.0°C – 26.0°C` |
| **Emergency Response** | `≤ 2 seconds` automated trigger latency for smoke venting and bathroom fall alarms |
| **Access Control** | 100% authorized entry enforcement via RFID (`Card ID = 1001`); unauthorized cards blocked |
| **Privacy-First Safety** | Non-optical elderly fall detection (`Arefd Guard` + `Fall Alarm`) with zero cameras |

### 3. Network Topology & Spatial Layout
The logical workspace models a 6-zone residential floor plan (**Server Room, Master Bedroom, Kitchen, Work Room, Toilet, Living Room**) plus an outdoor test trigger (`Old Car`), showing wireless connectivity across all 15 IoT endpoints and the central Home Gateway.

<p align="center">
  <img src="Images/IOT%20devices%20placement%20in%20House%28connected%29.jpg" alt="Smart Home IoT Connected Topology" width="90%" />
</p>

### 4. Network Architecture & IP Addressing (`192.168.25.0/24`)
| Device / Group | Model / Role | IP Assignment | Security / Notes |
| :--- | :--- | :--- | :--- |
| **Home Gateway** | `DLC100` | `192.168.25.1` (Static) | WPA2-PSK (AES), local IoT Registration Server & rule engine |
| **Internal Server** | `Server-PT` | `192.168.25.2` (Static) | Centralized local service support |
| **IoT Endpoints (15)** | Sensors & Actuators | `192.168.25.3 – 192.168.25.254` (DHCP) | Wireless association via Home Gateway |

### 5. Automation Logic (11 Closed-Loop IF-THEN Rules)
All 11 closed-loop automation rules are configured on the Home Gateway IoT Server (`http://192.168.25.1/conditions.html`):

<p align="center">
  <img src="Images/Conditions.png" alt="IoT Server Automation Conditions" width="85%" />
</p>

| Subsystem | Rules Configured | Trigger & Automated Action |
| :--- | :--- | :--- |
| **Climate Control** | `AC_ON`, `AC_OFF`, `Heater_Auto_On`, `Heater_Auto_OFF` | Regulates smart AC and furnace automatically to maintain `18.0°C – 26.0°C` |
| **Smart Lighting** | `Motion_Light_ON`, `Motion_Light_OFF` | Switches room lighting on/off based on motion sensor occupancy |
| **Fire & Smoke Safety** | `Smoke_Open_Window`, `Smoke_Off_Window` | Opens smart window/ventilation upon smoke detection and resets when clear |
| **RFID Access Control** | `Open_Door`, `Close_Door` | Unlocks smart door strictly for authorized `Card ID = 1001` and relocks automatically |
| **Elderly Fall Safety** | `Toilet_Fall_Detector` | Triggers `Fall Alarm` immediately upon bathroom fall-sensor activation |

### 6. Verification & Test Results
All **10 functional test scenarios** (high/low temperature HVAC switching, smoke detection & venting, authorized vs. unauthorized RFID scans, motion lighting on/off, and bathroom fall alarm) passed end-to-end verification (`10/10 Pass`). Complete screenshots and test logs are available in [`IOT SMART HOME.pdf`](IOT%20SMART%20HOME.pdf).

### 7. Engineering Challenges & Troubleshooting
- **IoT Device Registration**: Resolved endpoints not appearing on the Home Gateway dashboard by verifying WPA2-PSK credentials, DHCP IP lease assignment, and setting the Remote Server mode to **Home Gateway**.
- **Actuator State Persistence**: Prevented actuators from staying locked in an active state after trigger clearance by pairing every `ON` rule with an explicit opposing `OFF` / reset rule.
- **Rule Conflict Avoidance**: Structured non-overlapping temperature thresholds (`18.0°C` lower bound vs. `26.0°C` upper bound) so the heater and AC never compete simultaneously.

### 8. How to Run & Replicate
1. Install **Cisco Packet Tracer v8.0+**.
2. Download and open [`Smart_Home_IoT.pkt`](Smart_Home_IoT.pkt).
3. Click the **Tablet** → **Desktop** → **Web Browser** → open `http://192.168.25.1` (Credentials: `admin` / `admin`).
4. View live device telemetry under **Home** and inspect all 11 rules under **Conditions**.
5. Hold `Alt + Click` on sensors/triggers (e.g., Motion Detector, RFID Card, `Old Car` for smoke) to test real-time responses.

### 9. Repository Structure
```text
smart-home-iot-packet-tracer/
├── README.md
├── Smart_Home_IoT.pkt                                      # Cisco Packet Tracer simulation file
├── IOT SMART HOME.pdf                                      # Full technical report & test evidence
└── Images/
    ├── Conditions.png                                      # 11 IF-THEN automation rules screenshot
    ├── Empty Floor Plan.png                                # Base 6-zone residential floor plan
    ├── IOT devices placement in House.jpg                  # Device placement layout (pre-link)
    └── IOT devices placement in House(connected).jpg       # Full connected IoT network topology
```

---

<a id="中文版本"></a>
## 中文版本

### 1. 项目概述
本项目是基于 **Cisco Packet Tracer** 构建并验证的集中式边缘计算 **智能家居物联网与安防自动化系统**。系统通过 Home Gateway（`DLC100`）与局域网子网 `192.168.25.0/24` 连接分布在 **6 个家庭功能区** 的 **15 个 IoT 终端设备**，实现温控节能、人体感应照明、RFID 门禁、烟雾自动排风以及无摄像头隐私保护型老人跌倒报警。

### 2. 核心性能指标（KPI）
- **节能与舒适度**：日常空调/照明能耗预计降低 20–30%，室内温度自动维持在 `18.0°C – 26.0°C`。
- **应急响应速度**：烟雾排风与卫生间跌倒报警触发延迟 `≤ 2 秒`。
- **门禁安全**：授权 RFID 卡片（`Card ID = 1001`）100% 精准校验放行，未授权卡自动拦截。
- **隐私保护**：卫生间采用非光学跌倒检测（`Arefd Guard` + `Fall Alarm`），无需摄像头即可告警。

### 3. 网络拓扑与空间布局
<p align="center">
  <img src="Images/IOT%20devices%20placement%20in%20House%28connected%29.jpg" alt="智能家居 IoT 连线拓扑图" width="90%" />
</p>

### 4. 网络架构与 IP 规划（`192.168.25.0/24`）
| 设备 / 分组 | 型号 / 角色 | IP 分配 | 安全与说明 |
| :--- | :--- | :--- | :--- |
| **Home Gateway** | `DLC100` | `192.168.25.1`（静态） | WPA2-PSK (AES)，本地 IoT 注册服务器与规则引擎 |
| **Internal Server** | `Server-PT` | `192.168.25.2`（静态） | 本地集中服务支持 |
| **15 个 IoT 终端** | 传感器与执行器 | `192.168.25.3 – 192.168.25.254`（DHCP） | 通过 Home Gateway 无线接入 |

### 5. 自动化触发逻辑（11 条闭环 IF-THEN 规则）
<p align="center">
  <img src="Images/Conditions.png" alt="IoT 自动化规则面板截图" width="85%" />
</p>

涵盖温控（`AC_ON` / `AC_OFF` / `Heater_Auto_On` / `Heater_Auto_OFF`）、人体感应照明（`Motion_Light_ON` / `Motion_Light_OFF`）、烟雾开窗排风（`Smoke_Open_Window` / `Smoke_Off_Window`）、RFID 门禁（`Open_Door` / `Close_Door`）及卫生间跌倒报警（`Toilet_Fall_Detector`），全部 10 项功能测试用例均验证通过（完整报告见 [`IOT SMART HOME.pdf`](IOT%20SMART%20HOME.pdf)）。

### 6. 复现步骤
1. 使用 **Cisco Packet Tracer v8.0+** 打开根目录下的 [`Smart_Home_IoT.pkt`](Smart_Home_IoT.pkt)。
2. 点击场景中的 **Tablet** → **Desktop** → **Web Browser**，访问 `http://192.168.25.1`（账号密码均为 `admin`）。
3. 按住 `Alt + 左键点击` 传感器或触发器（如 `Old Car` 产生烟雾、刷 RFID 卡）即可实时测试联动效果。

[返回顶部 / Back to Top](#smart-home-iot-automation--security-system-cisco-packet-tracer)
