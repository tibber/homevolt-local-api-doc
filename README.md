# Homevolt Local API – Developer Overview

The Homevolt Local API is a **device-hosted HTTP/REST API** exposed by the Homevolt battery's embedded ECU (Energy Control Unit). It provides **direct, low-latency access over the local network** for monitoring, configuration, and control of the Homevolt system.

This API is intended **exclusively for local integrations**, such as Home Assistant, Node-RED, custom scripts and dashboards etc.

The API runs entirely on the Homevolt device and is accessed via its local hostname
(e.g. `homevolt-<deviceid>.local`) or LAN IP address.
All communication remains **within the local network** and does not depend on Tibber cloud services.

---

## Functional scope

Using the Local API, clients can:

- Read system and battery status (SOC, power, firmware, uptime)
- Monitor real-time energy and measurement data (grid, inverter, CTs)
- Configure system and EMS parameters
- Control battery behavior (e.g. minimum SOC, power limits, schedules)
- Retrieve diagnostics, logs, and node information

---

## Intended usage and limitations

- Designed for **local, on-premise control only**
- Not a public or cloud-hosted API
- Not intended for energy supplier, VPP, or third-party remote control

---

## ⚠️ Developer disclaimer (use at your own risk)

This API is provided for **advanced users and developers**.

By using the Homevolt Local API, you acknowledge that:

- You are responsible for how the API is used and integrated
- Incorrect usage or misconfiguration **may affect system behavior, performance, or availability**
- Tibber does **not** guarantee stability, support, or compatibility for custom integrations
- Future firmware updates may change or deprecate API behavior

**Recommendations:**

- Test integrations carefully in controlled environments
- Avoid writing to parameters you do not fully understand
- Prefer read-only access where possible

---

## API enablement requirement

> ⚠️ **The Local API is disabled by default.**

API access must be explicitly enabled on the Homevolt device.

To enable the API, the customer must contact **Tibber Customer Support**, who will verify eligibility and activate access.

Until the API is enabled:

- Webserver endpoints are unavailable
- Authentication credentials are inactive
- Local integrations (e.g. Home Assistant, Node-RED) cannot connect

## 📚 Documentation Files Overview

### REST API Documentation

#### 👀 I want to understand all endpoints
→ **[API_DOCUMENTATION.md](API_DOCUMENTATION.md)**
- Detailed explanations with examples
- Complete parameter descriptions
- Response examples for each endpoint
- Troubleshooting guide

#### ⚡ I need a quick API reference
→ **[API_QUICK_REFERENCE.md](API_QUICK_REFERENCE.md)**
- Endpoints organized by category
- Quick parameter lookup
- Common command examples
- Response structure patterns

#### 🧪 I want to test the API interactively
→ **[Tibber_ECU_API.postman_collection.json](Tibber_ECU_API.postman_collection.json)**
- Pre-configured Postman requests
- Built-in authentication setup
- Import and use immediately

#### 💻 I'm building an API client
→ **[API_DOCUMENTATION.yaml](API_DOCUMENTATION.yaml)**
- Complete OpenAPI 3.0 specification
- Use with code generators
- Schema definitions for all endpoints
- For integration with tools

#### 🗺️ I'm new to this API
→ **[API_DOCUMENTATION_README.md](API_DOCUMENTATION_README.md)**
- Navigation guide for all files
- Quick start instructions
- Integration examples
- Security best practices

### Console API Commands Documentation

#### 💻 I need console command documentation
→ **[CONSOLE_QUICK_REFERENCE.md](CONSOLE_QUICK_REFERENCE.md)** ⭐ **START HERE**
- Quick lookup table of all 50+ commands
- Log level reference
- Keyboard shortcuts
- Common recipes and quick tips
- **Best for**: Finding a command fast

#### 📖 I want detailed console command guide
→ **[CONSOLE_COMMANDS_REFERENCE.md](CONSOLE_COMMANDS_REFERENCE.md)**
- Complete documentation for all commands
- Arguments and subcommands
- Usage examples with output
- Common workflows (diagnostics, config, debugging)
- Troubleshooting section
- **Best for**: Learning how to use commands

#### 📋 I need a complete command inventory
→ **[CONSOLE_COMMANDS_INVENTORY.md](CONSOLE_COMMANDS_INVENTORY.md)**
- Comprehensive list of 80+ commands
- Organized by category
- File locations and function names
- Complete descriptions
- **Best for**: Reference and research

---

## 📊 File Summary Table

| File | Format | Size | Purpose | Best For |
|------|--------|------|---------|----------|
| **HTTP API Documentation** |
| API_DOCUMENTATION.md | Markdown | 18 KB | Complete reference | Detailed learning |
| API_QUICK_REFERENCE.md | Markdown | 8.5 KB | Cheat sheet | Quick lookup |
| API_DOCUMENTATION.yaml | OpenAPI 3.0 | 64 KB | Machine-readable spec | Code generation, tools |
| Tibber_ECU_API.postman_collection.json | JSON | 18 KB | Testing collection | Interactive testing |
| API_DOCUMENTATION_README.md | Markdown | 12 KB | Usage guide | Navigation |
| **Console API Command Documentation** |
| CONSOLE_QUICK_REFERENCE.md | Markdown | 6.8 KB | Quick lookup | Finding commands fast |
| CONSOLE_COMMANDS_REFERENCE.md | Markdown | 31 KB | Complete guide | Learning commands |
| CONSOLE_COMMANDS_INVENTORY.md | Markdown | 30 KB | Full inventory | Reference |
| **Navigation** |
| INDEX.md | Markdown | 9.2 KB | This file | Finding what you need |

---

## 🎯 Quick Links by Feature

### System Information & Diagnostics
**HTTP Endpoints**:
- [GET /status.json](API_DOCUMENTATION.md#get-statusjson) - System health
- [GET /logs.json](API_DOCUMENTATION.md#get-logsjson) - System logs

**Console Commands**:
- [`info`](CONSOLE_QUICK_REFERENCE.md) - Chip and SDK info
- [`tasks`](CONSOLE_QUICK_REFERENCE.md) - Running tasks
- [`dmesg`](CONSOLE_QUICK_REFERENCE.md) - System logs

### Battery & Energy Management
**HTTP Endpoints**:
- [GET /ems.json](API_DOCUMENTATION.md#get-emsjson) - Battery, solar, grid status
- [GET /ems_history.json](API_DOCUMENTATION.md#get-ems_historyjson) - Historical data
- [GET /schedule.json](API_DOCUMENTATION.md#get-schedulejson) - Charging schedule
- [GET /pid.json](API_DOCUMENTATION.md#get-pidjson) - PID controller state

**Console Commands**:
- [`ems`](CONSOLE_QUICK_REFERENCE.md) - EMS status
- [`sched_list`](CONSOLE_QUICK_REFERENCE.md) - View schedules
- [`sched_add`](CONSOLE_QUICK_REFERENCE.md) - Create schedule
- [`param_set`](CONSOLE_QUICK_REFERENCE.md) - Configure parameters

### Device Configuration
**HTTP Endpoints**:
- [GET /params.json](API_DOCUMENTATION.md#get-paramsjson) - List parameters
- [POST /params.json](API_DOCUMENTATION.md#post-paramsjson) - Set parameter

**Console Commands**:
- [`param_list`](CONSOLE_QUICK_REFERENCE.md) - List all parameters
- [`param_get`](CONSOLE_QUICK_REFERENCE.md) - Get value
- [`param_set`](CONSOLE_QUICK_REFERENCE.md) - Set value
- [`param_store`](CONSOLE_QUICK_REFERENCE.md) - Save to flash

### Network Nodes
**HTTP Endpoints**:
- [GET /nodes.json](API_DOCUMENTATION.md#get-nodesjson) - List nodes
- [GET /node_data.json](API_DOCUMENTATION.md#get-node_datajson) - Node electrical data
- [GET /node_metrics.json](API_DOCUMENTATION.md#get-node_metricsjson) - Node metrics

**Console Commands**:
- [`ecu_sense_list`](CONSOLE_QUICK_REFERENCE.md) - List sensors
- [`ecu_sense_assign`](CONSOLE_QUICK_REFERENCE.md) - Assign sensor

### Electrical Measurements
**HTTP Endpoints**:
- [GET /ct.json](API_DOCUMENTATION.md#get-ctjson) - Clamp measurements
- [GET /ct_history.json](API_DOCUMENTATION.md#get-ct_historyjson) - Clamp history
- [GET /mains_data.json](API_DOCUMENTATION.md#get-mains_datajson) - Voltage/frequency

**Console Commands**:
- [`modbus_read`](CONSOLE_QUICK_REFERENCE.md) - Read registers
- [`log_set`](CONSOLE_QUICK_REFERENCE.md) - Enable measurement logging

### Debugging & Troubleshooting
**HTTP Endpoints**:
- [GET /logs.json](API_DOCUMENTATION.md#get-logsjson) - System logs
- [GET /errors.json](API_DOCUMENTATION.md#get-errorsjson) - Error history

**Console Commands**:
- [`dmesg`](CONSOLE_QUICK_REFERENCE.md) - Display log buffer
- [`log_list`](CONSOLE_QUICK_REFERENCE.md) - List loggers
- [`log_set`](CONSOLE_QUICK_REFERENCE.md) - Change log level
- [`heap_trace_dump`](CONSOLE_QUICK_REFERENCE.md) - Memory analysis
- [`coredump_crash`](CONSOLE_QUICK_REFERENCE.md) - Generate crash dump

### Network & Connectivity
**HTTP Endpoints**:
- Network configuration endpoints
- WiFi status endpoints

**Console Commands**:
- [`ecu_network`](CONSOLE_QUICK_REFERENCE.md) - Network status
- [`lte_info`](CONSOLE_QUICK_REFERENCE.md) - Modem info
- [`lte_power`](CONSOLE_QUICK_REFERENCE.md) - Control modem
- [`lte_band`](CONSOLE_QUICK_REFERENCE.md) - Change band

### OTA Updates
- [GET /ota_manifest.json](API_DOCUMENTATION.md#get-ota_manifestjson) - Available updates
- [POST /update](API_DOCUMENTATION.md#post-update) - Upload firmware

### File Management
- [GET /spiffs/{filepath}](API_DOCUMENTATION.md#get-spiffsfilepath) - Download file
- [POST /upload/spiffs/{filepath}](API_DOCUMENTATION.md#post-uploadspiffsfilepath) - Upload file
- [POST /delete/spiffs/{filepath}](API_DOCUMENTATION.md#post-deletespiffsfilepath) - Delete file

### Debugging
- [POST /console.json](API_DOCUMENTATION.md#post-consolejson) - Execute CLI command
- [GET /error_report.json](API_DOCUMENTATION.md#get-error_reportjson) - Current errors

---

## 🚀 Getting Started (2 minutes)

1. **Import to Postman**
   ```
   Postman → Import → Tibber_ECU_API.postman_collection.json
   Set variables: hostname=homevolt-<deviceid>.local, password=YOUR_PASSWORD
   ```

2. **Test an endpoint**
   ```bash
   curl -u admin:<password> http://homevolt-<deviceid>.local/status.json
   ```

3. **View detailed docs**
   - Start with [API_DOCUMENTATION_README.md](API_DOCUMENTATION_README.md)
   - Then explore [API_DOCUMENTATION.md](API_DOCUMENTATION.md)

---

## 📖 Learning Paths

### Path 1: Quick Testing (15 minutes)
1. Read: DOCUMENTATION_SUMMARY.md (this section)
2. Import: Tibber_ECU_API.postman_collection.json
3. Try: GET /status.json
4. Reference: API_QUICK_REFERENCE.md as needed

### Path 2: Complete Integration (1 hour)
1. Read: API_DOCUMENTATION_README.md
2. Study: API_DOCUMENTATION.md
3. Reference: API_QUICK_REFERENCE.md
4. Code: Use integration examples

### Path 3: Code Generation (30 minutes)
1. Get: API_DOCUMENTATION.yaml
2. Generate: Client code with OpenAPI tools
3. Reference: API_DOCUMENTATION.md for details
4. Integrate: Use generated client

---

## 🛠️ Common Tasks

| Task | How To | Reference |
|------|--------|-----------|
| Check battery level | GET /ems.json, read battery_soc | API_DOCUMENTATION.md |
| List nodes | GET /nodes.json | API_QUICK_REFERENCE.md |
| Set a parameter | POST /params.json with k, v | API_DOCUMENTATION.md |
| Download logs | GET /logs.json | API_QUICK_REFERENCE.md |
| Execute CLI command | POST /console.json with cmd | API_DOCUMENTATION.md |
| Test an endpoint | Import Postman collection | Tibber_ECU_API.postman_collection.json |

---

## 🔗 API Base URLs

| Protocol | URL |
|----------|-----|
| HTTP | http://homevolt-<deviceid>.local |
| HTTPS | https://homevolt-<deviceid>.local |
| IP Address | http://192.168.x.x |

---

## 🔐 Authentication

All endpoints (except captive portal) require:
```
Username: admin
Password: <password>
```

Example:
```bash
curl -u admin:my-long-password http://homevolt-<deviceid>.local/ems.json
```

---

## ❓ Frequently Asked Questions

**Q: Which file should I read first?**
A: Start with [DOCUMENTATION_SUMMARY.md](DOCUMENTATION_SUMMARY.md), then choose based on your needs.

**Q: How do I test endpoints?**
A: Import [Tibber_ECU_API.postman_collection.json](Tibber_ECU_API.postman_collection.json) into Postman.

**Q: Where are example requests?**
A: [API_DOCUMENTATION.md](API_DOCUMENTATION.md) has curl examples for each endpoint.

**Q: How do I generate an API client?**
A: Use [API_DOCUMENTATION.yaml](API_DOCUMENTATION.yaml) with OpenAPI generators.

**Q: What if I get 401 errors?**
A: Check [API_DOCUMENTATION.md - Troubleshooting](API_DOCUMENTATION.md).

**Q: Where are the full endpoint parameters?**
A: See [API_DOCUMENTATION.md](API_DOCUMENTATION.md) for detailed parameter documentation.

---

## 📱 For Mobile/App Development

1. Download [API_DOCUMENTATION.yaml](API_DOCUMENTATION.yaml)
2. Generate client code:
   ```bash
   openapi-generator generate -i API_DOCUMENTATION.yaml -g swift
   # or kotlin, typescript, etc.
   ```
3. Use generated client in your app
4. Reference [API_DOCUMENTATION.md](API_DOCUMENTATION.md) for usage details

---

## 🖥️ For System Administration

1. Reference: [API_QUICK_REFERENCE.md](API_QUICK_REFERENCE.md)
2. Use endpoints:
   - /params.json - Configuration
   - /console.json - CLI access
   - /logs.json - Log monitoring
   - /error_report.json - Error tracking

---

## 🏠 For Home Automation (Node-RED, Home Assistant)

1. Import: [Tibber_ECU_API.postman_collection.json](Tibber_ECU_API.postman_collection.json)
2. Test: Verify connectivity
3. Reference: [API_QUICK_REFERENCE.md](API_QUICK_REFERENCE.md) for endpoint names
4. Integrate: Use HTTP request nodes with endpoints

---

## 🔄 For DevOps/Monitoring

1. Study: [API_DOCUMENTATION.md](API_DOCUMENTATION.md) - Energy Management section
2. Key endpoints:
   - /ems.json - Current status
   - /ems_history.json - Historical data
   - /error_report.json - Alerts
   - /logs.json - System logs
3. Set up: Monitoring/alerting based on these endpoints

---

## 📞 Need Help?

1. **Endpoint not found?** → Check [API_QUICK_REFERENCE.md](API_QUICK_REFERENCE.md)
2. **Need examples?** → See [API_DOCUMENTATION.md](API_DOCUMENTATION.md)
3. **Want to test?** → Use [Tibber_ECU_API.postman_collection.json](Tibber_ECU_API.postman_collection.json)
4. **Building integration?** → Reference [API_DOCUMENTATION_README.md](API_DOCUMENTATION_README.md)
5. **Getting errors?** → Check [API_DOCUMENTATION.md - Troubleshooting](API_DOCUMENTATION.md)

---

**Last Updated:** December 18, 2025

**API Version:** 1.0.0

**Device:** Tibber ECU
