# Tibber ECU - Console Commands Reference

This document provides comprehensive documentation of all console commands available on the Tibber ECU firmware.

## Table of Contents

1. [Quick Start](#quick-start)
2. [System Information Commands](#system-information-commands)
3. [Task & System Monitoring](#task--system-monitoring)
4. [Time & Clock Commands](#time--clock-commands)
5. [Memory Management](#memory-management)
6. [Logging Control](#logging-control)
7. [Filesystem Operations](#filesystem-operations)
8. [Parameter Management](#parameter-management)
9. [Crash Dump Management](#crash-dump-management)
10. [Energy Management System](#energy-management-system)
11. [Schedule Management](#schedule-management)
12. [ECU Control Commands](#ecu-control-commands)
13. [Sensor Assignment](#sensor-assignment)
14. [LTE Modem Control](#lte-modem-control)
15. [Common Workflows](#common-workflows)

---

## Quick Start

### Accessing the Console

Access the console via the HTTP API endpoint:
- **Endpoint**: `POST /console.json`
- **Authentication**: HTTP Basic Auth (admin:<password>)
- **Base URL**: `http://homevolt-<deviceid>.local` or `https://homevolt-<deviceid>.local`

Example using curl:
```bash
curl -u admin:<password> -X POST \
  -H "Content-Type: application/json" \
  -d '{"cmd":"info"}' \
  http://homevolt-<deviceid>.local/console.json
```

### Console Features
- **Help**: Most commands support `help command` or `help` to list available commands.

---

## System Information Commands

### `info`
**Description**: Get comprehensive information about the chip and SDK

**Help Text**: "Get info of chip and SDK"

**Usage**:
```
> info
```

**Output**: Displays detailed information including:
- Project name and version
- Bootloader version and date
- OTA manifest version
- Firmware version and reset reason
- Board revision and hardware configuration
- Uptime in seconds
- Device model, cores, and features
- Flash size and OTA partition information
- EFR hub ID and version
- Burn-in QR code and manufacturing info
- Network status (WiFi, MQTT, LTE)
- SIM card ID (if LTE available)
- WiFi MAC address

**Example Output**:
```
# Version
project.name: tibber-ecu
project.version: 1.0.0
device.cores: 2
device.flash.size: 16777216
uptime: 86400
wifi.sta.mac: aa:bb:cc:dd:ee:ff
```

---

### `reset_hard`
**Description**: Reset the device by pulling HMI_BTN1 high and waiting for hardware reset

**Help Text**: "Reset the device by pulling HMI_BTN1 high, and wait for hardware to reset"


**Usage**:
```
> reset_hard
```

**Details**: Triggers a hardware reset by pulling GPIO_48 (HMI_BTN1) high. The device will wait for the hardware to detect and perform the reset.

**Note**: This is different from a software reset - it resets via the physical button.

---


## Task & System Monitoring

### `echo`
**Description**: Print a string to console output

**Help Text**: "Print a string"


**Usage**:
```
> echo Hello World
Hello World
```

**Arguments**:
- `<string>`: Text to echo (can include multiple words)

**Examples**:
```
> echo test
test

> echo "Hello World"
Hello World
```

---

### `tasks`
**Description**: Get information about running system tasks

**Help Text**: "Get information about running tasks"


**Usage**:
```
> tasks
```


**Output**: Table showing:
- Task Name
- Status (Running/Blocked/etc)
- Priority
- High Water Mark (HWM) - remaining stack
- Task number/address
- CPU core affinity (if available)

**Example Output**:
```
Task Name          Status  Prio  HWM   Task#
main               X       1     1000  1
wifi_task          R       20    500   2
system_timer       R       22    2000  3
```

---

## Time & Clock Commands

### `time`
**Description**: Get local date/time information

**Help Text**: "Get local date/time information"


**Usage**:
```
> time                    # Show all (date, uptime, ticks)
> time date               # Show only current date/time
> time uptime             # Show system uptime
> time ticks              # Show system tick count
```

**Output Examples**:
```
> time date
The current date/time is: 2024-12-18 14:30:45

> time uptime
uptime: 1 days 2h 30m (95400)

> time ticks
ticks: 1234567890
```

---

### `history`
**Description**: Print command history from current session

**Help Text**: "Print command history"


**Usage**:
```
> history
```


**Output**: List of previously executed commands in the current console session

---

## Memory Management

### `free`
**Description**: Show available memory statistics

**Help Text**: "Show available memory"


**Usage**:
```
> free
```

**Output**: Memory usage information including:
- Total heap size
- Free memory
- Allocated memory
- Largest free block
- Memory fragmentation

---


## Logging Control

### `dmesg`
**Description**: Display system log buffer

**Help Text**: "Display system log buffer"


**Usage**:
```
> dmesg
```

**Output**: Shows recent system log messages with timestamps

---

### `log_list`
**Description**: List all loggers and their current log levels

**Help Text**: "List all loggers and their current log levels"


**Arguments**:
- `[search]`: Optional filter string to search tag names

**Usage**:
```
> log_list                  # Show all loggers
> log_list mqtt             # Show loggers matching "mqtt"
> log_list wifi             # Show WiFi-related loggers
```

**Output**: Table with columns:
- Logger name (tag)
- Current log level
- Module/component

**Example Output**:
```
mqtt           : INFO
wifi           : WARN
ota            : DEBUG
```

---

### `log_set`
**Description**: Set log level for a specific tag

**Help Text**: "Set log level"


**Arguments**:
- `<name>`: Log tag name (use `*` for all tags)
- `<level>`: Log level number:
  - `0` = NONE (no logging)
  - `1` = ERROR
  - `2` = WARN
  - `3` = INFO (default)
  - `4` = DEBUG
  - `5` = VERBOSE

**Usage**:
```
> log_set mqtt 4            # Set MQTT logger to DEBUG
> log_set * 3               # Set all loggers to INFO
> log_set wifi 5            # Set WiFi logger to VERBOSE
> log_set battery 1         # Set battery logger to ERROR only
```

**Useful Combinations**:
```
# Debug specific subsystem
> log_set * 2               # Global WARNING level
> log_set mqtt 4            # MQTT to DEBUG (verbose)

# Reduce noise
> log_set * 1               # Only errors
> log_set ota 3             # OTA at INFO
```

---

### `log_reset`
**Description**: Reset all log levels to default

**Help Text**: "Reset all log to loglevel"


**Arguments**:
- `[level]`: Optional default log level (defaults to configured level)

**Usage**:
```
> log_reset                 # Reset to default configuration
> log_reset 3               # Reset all to INFO level
> log_reset 2               # Reset all to WARN level
```

---

## Filesystem Operations

### `ls`
**Description**: List files in directory

**Help Text**: "list files"


**Arguments**:
- `<path>`: Directory path to list

**Usage**:
```
> ls /                      # List root directory
> ls /spiffs                # List SPIFFS partition
> ls /data                  # List data directory
```

**Output**: Shows files and directories with:
- File/directory name
- Size (for files)
- Type indicator (file vs directory)

---

### `cat`
**Description**: Display or create file contents

**Help Text**: "cat file"


**Arguments**:
- `<path>`: File path
- `[data]`: Optional data to write (creates file if doesn't exist)

**Usage**:
```
> cat /spiffs/config.json           # Display file contents
> cat /spiffs/newfile.txt content   # Create new file with content
> cat /spiffs/params.json           # Read JSON config
```

**Examples**:
```
> cat /spiffs/config.json
{"mode": "charge", "rate": 16}

> cat /spiffs/test.txt "Hello World"
# Creates file with content "Hello World"
```

---

### `sha1`
**Description**: Calculate SHA1 hash of a file

**Help Text**: "sha1 file"


**Arguments**:
- `<path>`: Path to file to hash

**Usage**:
```
> sha1 /spiffs/firmware.bin
> sha1 /spiffs/config.json
```

**Output**: SHA1 checksum in hexadecimal format

**Example Output**:
```
> sha1 /spiffs/config.json
da39a3ee5e6b4b0d3255bfef95601890afd80709
```

---

### `rm`
**Description**: Remove a file or directory

**Help Text**: "rm file"


**Arguments**:
- `<path>`: Path to remove

**Usage**:
```
> rm /spiffs/oldfile.txt
> rm /spiffs/temp_config.json
> rm /data/cache
```

**Note**: Removing directories may require directory to be empty first

---

### `format`
**Description**: Format the filesystem

**Help Text**: "Format the filesystem, this wont factory reset the device, but resources like local webserver needs to be downloaded by ota before starting to work again."


**Usage**:
```
> format
```

**Warning**: This command **erases all files on the SPIFFS filesystem**. Use with caution!

**Warning**: This command **erases all files on the SPIFFS filesystem**. Use with caution!

**Confirmation**: Typically no confirmation prompt, so be careful with command entry.

---

## Parameter Management

### `param_list`
**Description**: List all system parameters and their values

**Help Text**: "List stored key-value pairs"


**Arguments**:
- `[search]`: Optional filter string

**Usage**:
```
> param_list                    # Show all parameters
> param_list battery            # Show battery-related parameters
> param_list charge             # Show charge-related parameters
```

**Output**: Shows parameter names, current values, types, and ranges

**Example Output**:
```
battery_mode            : 1          (range: 0-3)
max_charge_current      : 16         A (range: 0-32)
discharge_efficiency    : 0.95       % (range: 0.0-1.0)
```

---

### `param_get`
**Description**: Get current value of a specific parameter

**Help Text**: "Get param value"


**Arguments**:
- `<name>`: Parameter name

**Usage**:
```
> param_get battery_mode
1

> param_get max_charge_current
16
```

---

### `param_set`
**Description**: Set parameter to a new value

**Help Text**: "Set key-value pair in selected namespace"


**Arguments**:
- `<name>`: Parameter name
- `<value>`: New value

**Usage**:
```
> param_set battery_mode 2
> param_set max_charge_current 20
> param_set discharge_efficiency 0.92
```

**Important**: Changes are stored in memory but NOT persisted to flash until `param_store` is executed

**Validation**: Most parameters are validated - invalid values will be rejected

---

### `param_reset`
**Description**: Reset parameter to default value

**Help Text**: "Reset param to default value"


**Arguments**:
- `[name]`: Optional specific parameter to reset

**Usage**:
```
> param_reset                   # Reset ALL parameters to defaults
> param_reset battery_mode      # Reset only battery_mode
> param_reset max_charge_current # Reset only this parameter
```

**Note**: This resets to the compiled defaults, not the stored values

---

### `param_store`
**Description**: Store all parameter changes to flash

**Help Text**: "Store all param changes to flash, so the persist over reboot"


**Usage**:
```
> param_store
```

**Details**: Writes current parameter values to non-volatile storage (NVS flash). Must be called to persist changes made with `param_set`.

**Workflow**:
```
> param_set battery_mode 2      # Change in memory
> param_set max_charge 20       # Change in memory
> param_store                   # Save to flash
# Device restart will remember these values
```

---

### `param_load`
**Description**: Load all parameters from flash

**Help Text**: "Load all param changes from flash"


**Usage**:
```
> param_load
```

**Details**: Reloads all parameters from persistent storage, discarding any unsaved changes made with `param_set`.

**Useful for**: Reverting to last saved configuration

---

## Energy Management System

### `ems`
**Description**: Show Energy Management System status

**Help Text**: "Show EMS system status"


**Usage**:
```
> ems
```

**Output**: Displays:
- Battery state (charge/discharge/idle)
- Power flow (solar, battery, grid)
- Energy levels
- System efficiency
- Active limits and modes

---

### `ems_reset`
**Description**: Reset EMS system to initial state

**Help Text**: "Reset EMS state"


**Usage**:
```
> ems_reset
```

**Details**: Resets all EMS internal state machines and counters

---

### `ems_config_set`
**Description**: Set EMS configuration parameters

**Help Text**: "Set EMS configuration"


**Arguments**:
- `-g`: grid code
- `-t`: control timeout


---

## Schedule Management

### `sched_add`
**Description**: Add a new energy schedule

**Help Text**: "Add a new schedule"


**Usage**:

See: [BATTERY_CONTROL_GUIDE.md](BATTERY_CONTROL_GUIDE.md)


### `sched_set`
**Description**: Modify an existing schedule

**Help Text**: "Modify an existing schedule"


**Usage**:

See: [BATTERY_CONTROL_GUIDE.md](BATTERY_CONTROL_GUIDE.md)

### `sched_del`
**Description**: Delete a schedule

**Help Text**: "Delete a schedule"


**Usage**:

See: [BATTERY_CONTROL_GUIDE.md](BATTERY_CONTROL_GUIDE.md)

### `sched_list`
**Description**: List all configured schedules

**Help Text**: "List all schedules"


**Usage**:
See: [BATTERY_CONTROL_GUIDE.md](BATTERY_CONTROL_GUIDE.md)


---

### `sched_clear`
**Description**: Clear all configured schedules

**Help Text**: "Clear all schedules"


**Usage**:

See: [BATTERY_CONTROL_GUIDE.md](BATTERY_CONTROL_GUIDE.md)

---

## ECU Control Commands

### `ecu_ffr_rearm`
**Description**: Rearm FFR (Force Feed Rearm) if in standby

**Help Text**: "Rearm FFR immediately if in standby"


**Usage**:
```
> ecu_ffr_rearm
```

**Details**: Resets the Force Feed Rearm state, allowing immediate system restart

---

### `ecu_network`
**Description**: Show secure transport status

**Help Text**: "Show secure transport status"


**Usage**:
```
> ecu_network
```

**Output**: Displays network connectivity status including:
- Encryption status
- Transport layer health
- Connection reliability
- Pending commands/messages

---

### `ecu_network_reset`
**Description**: Reinitialize secure network transport

**Help Text**: "Reinitialize secure transport"


**Usage**:
```
> ecu_network_reset
```

**Details**: Tears down and reinitializes the secure network transport layer. Useful for recovering from network issues.

---

## Sensor Assignment

### `ecu_sense_assign`
**Description**: Assign a wireless sensor node to a function

**Help Text**: "Assign a sensor node to a function"


**Arguments**:
- `<node_id>`: Node ID to assign
- `<function>`: Function type (e.g., power, temperature, humidity)
- `[-i/--interface]`: Optional interface specification

**Usage**:
```
> ecu_sense_assign node_001 power
> ecu_sense_assign node_002 temperature -i L1
> ecu_sense_assign node_003 voltage
```

---

### `ecu_sense_clear`
**Description**: Clear sensor node assignments

**Help Text**: "Clear sensor assignments"


**Arguments**:
- `[node_id]`: Optional specific node to clear

**Usage**:
```
> ecu_sense_clear              # Clear ALL assignments
> ecu_sense_clear node_001     # Clear specific node
```

---

### `ecu_sense_list`
**Description**: List all sensor assignments

**Help Text**: "List all sensor assignments"


**Usage**:
```
> ecu_sense_list
```

**Output**: Shows all sensor nodes and their assigned functions

**Example Output**:
```
Node ID    Function      Interface  Status
node_001   power         L1-L2      ACTIVE
node_002   temperature   -          ACTIVE
node_003   voltage       L1         INACTIVE
```

---

## LTE Modem Control

### `lte_info`
**Description**: Show LTE modem information

**Help Text**: "Show LTE modem information"


**Usage**:
```
> lte_info
```

**Output**: Displays:
- Modem power state
- Network registration status
- Signal strength (RSSI, RSRP)
- Connected network info
- IMEI and firmware version
- Supported bands
- Current band

---

### `lte_power`
**Description**: Control LTE modem power state

**Help Text**: "Control LTE modem power"


**Arguments**:
- `<power>`: 1 = Power ON, 0 = Power OFF

**Usage**:
```
> lte_power 1       # Turn modem ON
> lte_power 0       # Turn modem OFF
```

---

### `lte_restart`
**Description**: Restart the LTE modem

**Help Text**: "Restart LTE modem"


**Usage**:
```
> lte_restart
```

**Details**: Performs complete reset and restart of the modem. Useful for recovering from connection issues.

---

### `lte_band`
**Description**: Change LTE network band

**Help Text**: "Change network band"


**Arguments**:
- `<band>`: Band number (e.g., 3, 7, 20, 28, 32)

**Usage**:
```
> lte_band 20       # Switch to Band 20
> lte_band 7        # Switch to Band 7
```

**Common Bands**:
- Band 3: 1800 MHz
- Band 7: 2600 MHz
- Band 20: 800 MHz (better indoor)
- Band 28: 700 MHz (excellent indoor)
- Band 32: 1500 MHz

---

### `lte_sim_iccid`
**Description**: Get SIM card ICCID

**Help Text**: "Get SIM ICCID"


**Usage**:
```
> lte_sim_iccid
```

**Output**: 20-digit ICCID of the inserted SIM card

**Format**: International standard ICCID format

---

## Common Workflows

### System Diagnostics Workflow

```bash
# 1. Get basic system info
> info

# 2. Check hardware health
> free                          # Memory status
> tasks                         # Running tasks
> time uptime                   # System uptime

# 3. Check logs for errors
> log_list
> log_set * 2                   # Set all logs to WARN level
> dmesg                         # View log buffer

# 4. If issues found, increase debug level
> log_set webserver 4           # Set all logs to DEBUG
> dmesg                         # Check detailed logs

# 5. Check specific subsystem
> log_list                      # Find MQTT logger
> log_set mqtt 4                # Set MQTT to DEBUG
```

### Configuration Management Workflow

```bash
# 1. List current parameters
> param_list

# 2. Get specific value
> param_get settings_local

# 3. Make changes
> param_set settings_local false
> param_set ecu_main_fuse_size_a 20

# 4. Verify changes
> param_get settings_local        # Should be false

# 5. Save to persistent storage
> param_store

```

### Energy Management Workflow

See: [BATTERY_CONTROL_GUIDE.md](BATTERY_CONTROL_GUIDE.md)


---

## Additional Resources

- [API_DOCUMENTATION.md](API_DOCUMENTATION.md) - HTTP/WebSocket API reference
- [PARAMETERS_REFERENCE.md](PARAMETERS_REFERENCE.md) - Parameter documentation
- [BATTERY_CONTROL_GUIDE.md](BATTERY_CONTROL_GUIDE.md) - Battery control examples
- Modbus Protocol: https://en.wikipedia.org/wiki/Modbus
- LTE Bands: https://en.wikipedia.org/wiki/LTE_frequency_bands

