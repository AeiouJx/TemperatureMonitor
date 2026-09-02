# Temperature Monitor

<h1 align="center">
  <img src="./Image/头像草图.jpg" alt="Temperature Monitor" width="128" />
  <br>
  Temperature Monitor
  <br>
</h1>

<h3 align="center">
A cross-platform temperature monitoring application built with <a href="https://www.ni.com/">LabVIEW</a>, featuring Modbus communication, SQLite &amp; TDMS dual storage, and real-time graphing.
</h3>

<p align="center">
  <a href="#features">Features</a> •
  <a href="#architecture">Architecture</a> •
  <a href="#installation">Installation</a> •
  <a href="#development">Development</a> •
  <a href="#faq">FAQ</a> •
  <a href="#license">License</a>
</p>

---

## Features

- **Cross-Platform** — Supports Windows, macOS (11+), and Linux (x86_64)
- **Modbus Communication** — Connects to industrial temperature instruments (e.g., Acrel ARTM series) via Modbus RTU/TCP
- **Dual Data Storage** — Saves data to both SQLite (for structured queries) and TDMS (for high-performance time-series data)
- **Real-Time Graphing** — Displays temperature curves on XY graphs with mouse interaction
- **Historical Data Query** — Query and review past temperature records from the database
- **Configurable Sampling Rate** — Adjust data acquisition frequency to suit your needs
- **Device Configuration UI** — Form-based UI for managing communication settings
- **Touch-Screen Friendly** — Virtual keyboard support for tablet/kiosk deployments
- **Modular Architecture** — Built on the Delacor QMH (Queued Message Handler) framework for robust, maintainable multi-module design

---

## Architecture

The application follows a modular **Queued Message Handler (QMH)** pattern with four core modules:

```
Launcher.vi (Entry Point)
    └── Application Module — Main UI & coordinator
            ├── DAQ Module        — Modbus communication & data acquisition
            ├── Storage Module    — Data persistence (SQLite + TDMS)
            └── DataQuery Module  — Historical data retrieval
```

### Module Overview

| Module | Role | Key VIs |
|--------|------|---------|
| **Application** | Top-level controller, manages UI and module lifecycle | Show Panel, Hide Panel, Stop Module |
| **DAQ** | Communicates with temperature instruments via Modbus | 建立连接, 采集温度数据, Update Sampling Rate |
| **Storage** | Persists temperature data to SQLite and TDMS files | 存储Data至数据库, 存储Data至TDMS, Get TDMS Ref |
| **DataQuery** | Provides historical data query UI and logic | Form_HistoricalDataQuery |

### Key Components

| Component | Description |
|-----------|-------------|
| **Com Library** | Business logic VIs for Modbus communication, database operations, and data storage |
| **Basic Library** | Utility VIs for UI helpers, IP validation, mouse interactions, and module initialization |
| **FGV (Functional Global Variables)** | Shared data containers for inter-module communication |
| **SQLite Database** | `Data/FD.db` — stores temperature records and device configuration |
| **TDMS Files** | Runtime-generated files for high-speed time-series data |

---

## Hardware Compatibility

This project is developed and tested with the **Acrel ARTM Series Temperature Scanner** (安科瑞ARTM系列温度巡检仪).

| Item | Detail |
|------|--------|
| Device | Acrel ARTM8 (8-channel temperature scanner) |
| Protocol | Modbus RTU / Modbus TCP |
| Default IP | `192.168.80.1` |
| Manual | See `Document/安科瑞ARTM 系列温度巡检仪V1.3.pdf` |

The application can be extended to support other Modbus-compatible temperature instruments by modifying the DAQ module configuration.

---

## Installation

### Pre-built Binaries

Download the latest release from [GitHub Releases](https://github.com/GuiHuaX/TemperatureMonitor/releases). Available platforms:

- Windows x64
- Linux x86_64
- macOS 11+

### Build from Source

**Prerequisites:**

1. **LabVIEW 2018** or later — [Download](https://www.ni.com/zh-cn/support/downloads/software-products/download.labview.html#487445)
2. **SQLite Library** — [drjdpowell/SQLite Library](https://github.com/drjdpowell/SQLite-Library) (install via VIPM)
3. **Delacor QMH Toolkit** — Install via VI Package Manager (VIPM)
4. **Modbus Library** — Included with LabVIEW (Instrument I/O palette)

**Build Steps:**

1. Clone this repository
2. Open `Temperature Monitor/Temperature Monitor.lvproj` in LabVIEW
3. Ensure all dependencies are resolved in the project explorer
4. Build the application using the "My Application" build specification (Output: `应用程序.exe`)

> **Note:** If the application fails to start on Windows, ensure LabVIEW runtime is installed.

---

## Development

### Project Structure

```
TemperatureMonitor/
├── Document/                          Hardware documentation
│   ├── 安科瑞ARTM 系列温度巡检仪V1.3.pdf
│   └── ARTM8面板图示.png
├── Image/                             Project assets
├── Temperature Monitor/
│   ├── Launcher.vi                    Application entry point
│   ├── Data/FD.db                     SQLite database
│   ├── Com Library/                   Business logic VIs
│   ├── Basic Library/                 Utility/helper VIs
│   └── Libraries/                     QMH module libraries
│       ├── Application/               Main UI & controller module
│       ├── DAQ/                       Data acquisition module
│       ├── Storage/                   Data persistence module
│       └── DataQuery/                 Historical query module
```

### Adding a New Module

1. Duplicate an existing module folder under `Libraries/`
2. Rename the `.lvlib` file and update internal references
3. Define custom Request VIs and argument clusters in the `Requests/` subfolder
4. Implement the module's `Main.vi` following the QMH pattern
5. Register the module in the main project file (`Temperature Monitor.lvproj`)

### Code Conventions

- Module VIs follow the Delacor QMH naming pattern (Public API, Broadcasts, Requests, Private)
- Chinese VI names are used for domain-specific operations (e.g., `存储Data至数据库.vi`)
- English names are used for framework/utility VIs
- All modules share a common initialization and synchronization template

---

## FAQ

**Q: Some files could not be found / third-party libraries are missing**

A: Install the required packages via VI Package Manager (VIPM):
- SQLite Library (by drjdpowell)
- Delacor QMH Toolkit

**Q: How do I connect to a different Modbus device?**

A: Open the Device Communication Configuration form (`Form_设备通讯配置.vi`) to change the Modbus connection parameters (IP address, port, unit ID).

**Q: Where is the data stored?**

A: Temperature data is saved in two formats:
- **SQLite** — `Temperature Monitor/Data/FD.db` (for queries and reporting)
- **TDMS** — Runtime-generated `.tdms` files (for high-performance time-series data)

**Q: Can I run this on macOS or Linux?**

A: Yes. The application is built using LabVIEW, which supports cross-platform deployment to Windows, macOS, and Linux.

---

## License

This project is licensed under the **GNU General Public License v3.0**. See [LICENSE](./LICENSE) for details.

---

<div align="center">
  <img src="./Image/头像草图.jpg" alt="Temperature Monitor" width="64" />
  <br>
  <sub>Built with LabVIEW</sub>
</div>
