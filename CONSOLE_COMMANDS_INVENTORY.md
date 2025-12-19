# ESP Console Commands Inventory

Complete listing of all console commands available on the device.

## System Information Commands

### Command: `info`
- **Help**: Get info of chip and SDK
- **Arguments**: None
- **Description**: Displays comprehensive system information including firmware versions, update status, board configuration, hub status, burnin info, and network status.

### Command: `reset_hard`
- **Help**: Reset the device by pulling HMI_BTN1 high, and wait for hardware to reset
- **Arguments**: None
- **Description**: Performs a hardware reset by setting GPIO_NUM_48 high.

### Command: `event_dump`
- **Help**: Dump event loop info
- **Arguments**: None
- **Description**: Dumps event loop profiling information.

---

## Logging Commands

### Command: `dmesg`
- **Help**: Print log buffer
- **Arguments**:
  - `-c, --clear`: Clear buffer on receive
  - `-p, --purge`: Purge buffer without printing
  - `-s, --stats`: Print log buffer stats
  - `-l, --limit <limit>`: Maximum count of entries to fetch at a time, -1 for unlimited
- **Description**: Displays log buffer contents with optional clearing or purging.

### Command: `log_list`
- **Help**: Set log level
- **Arguments**:
  - `<search>`: Search for log tag name
- **Description**: Lists active log tags with their current log levels. Can filter by search term.

### Command: `log_set`
- **Help**: Set log level
- **Arguments**:
  - `<name>`: Log tag name
  - `<level>`: Log level (0=None, 1=Error, 2=Warn, 3=Info, 4=Debug, 5=Verbose)
- **Description**: Sets log level for a specific tag or wildcard pattern.

### Command: `log_reset`
- **Help**: Reset all log to loglevel
- **Arguments**:
  - `<level>`: Log level (0=None, 1=Error, 2=Warn, 3=Info, 4=Debug, 5=Verbose)
- **Description**: Resets all log tags to a specific log level.

---

## File System Commands

### Command: `ls`
- **Help**: list files
- **Arguments**:
  - `<path>`: Path to list files in (required)
- **Description**: Lists files and directories with their types and sizes.

### Command: `cat`
- **Help**: cat file
- **Arguments**:
  - `<path>`: File to cat (required)
  - `<data>`: Data to write (optional - if included, file is created)
- **Description**: Displays file contents or creates/writes to a file if data is provided.

### Command: `sha1`
- **Help**: sha1 file
- **Arguments**:
  - `<path>`: File to sha sum (required)
- **Description**: Calculates and displays SHA1 checksum of a file.

### Command: `rm`
- **Help**: rm file
- **Arguments**:
  - `<path>`: Path to remove (required)
- **Description**: Removes (deletes) a file from the filesystem.

### Command: `free`
- **Help**: shot spi disk usage
- **Arguments**: None
- **Description**: Shows SPI filesystem partition usage (total and used space).

### Command: `format`
- **Help**: Format the filesystem
- **Arguments**: None
- **Description**: Formats the filesystem.

---

## Memory and System Commands

### Command: `echo`
- **Help**: Print a string
- **Arguments**: `<string...>` - Any number of strings to echo
- **Description**: Echoes back the provided arguments to stdout.

### Command: `tasks`
- **Help**: Get information about running tasks
- **Arguments**: None
- **Description**: Lists all running tasks with their status, priority, and high water mark.

### Command: `time`
- **Help**: Get local time
- **Arguments**: `'uptime'`, `'date'`, `'ticks'` (optional - shows all if not specified)
- **Description**: Displays current date/time, system uptime, and tick count.

### Command: `history`
- **Help**: Print command history
- **Arguments**: None
- **Description**: (Conditional on command history feature) Prints command history from file.

### Command: `read_mem`
- **Help**: Read memory
- **Arguments**:
  - `memory_offset` (required): Memory address to read from
  - `size` (required): Number of bytes to read
  - `-b, --bin`: Read as binary
  - `-x, --hex`: Read as hex
  - `-c, --chr`: Read as character
  - `-d, --dec`: Read as decimal
- **Description**: Reads and displays memory contents in specified format.

### Command: `write_mem`
- **Help**: Write memory
- **Arguments**:
  - `memory_offset` (required): Memory address to write to
  - `value` (required): Value to write
  - `-b, --byte`: Write 8 bits
  - `-w, --word`: Write 16 bits
  - `-d, --dword`: Write 32 bits
  - `-q, --qword`: Write 64 bits
- **Description**: Writes values to memory addresses.

### Command: `mem`
- **Help**: Get the current size of free heap memory
- **Arguments**:
  - `-t, --tasks`: Dump task memory information
  - `-h, --heap`: Dump heap memory information
  - `-f, --full`: Dump full heap memory information
  - `-d, --dump`: Dump heap memory information
  - `-e, --dumpe`: Dump heap memory information
  - `-a, --all`: Dump all memory information
- **Description**: Displays memory usage including heap, PSRAM, and task information.

### Command: `tasks_load`
- **Help**: Get run-time stats of cpu load
- **Arguments**:
  - `<period_ms>` (optional): Sampling period in milliseconds (default: 1000)
- **Description**: Displays CPU load statistics for all tasks over time.

### Command: `heap_trace_dump`
- **Help**: Dump heap trace
- **Arguments**:
  - `--internal`: Dump internal heap trace
  - `--spiram`: Dump SPIRAM heap trace
- **Description**: (Conditional on heap tracing feature) Displays heap trace data.

### Command: `heap_trace_start`
- **Help**: Start heap trace
- **Arguments**:
  - `--leaks`: Trace heap leaks only
  - `--min_size <n>`: Minimum size of allocation to trace
- **Description**: (Conditional on heap tracing feature) Starts heap tracing.

### Command: `heap_trace_stop`
- **Help**: Stop heap trace
- **Arguments**: None
- **Description**: (Conditional on heap tracing feature) Stops heap tracing.

---

## Network Commands

### Command: `ping`
- **Help**: send ICMP ECHO_REQUEST to network hosts
- **Arguments**:
  - `<host>` (required): Host address (IP or hostname)
  - `-W, --timeout <t>`: Time to wait for response in seconds
  - `-i, --interval <t>`: Interval between packets in seconds
  - `-s, --size <n>`: Number of data bytes to send
  - `-c, --count <n>`: Number of packets to send
  - `-Q, --tos <n>`: Type of Service bits
  - `-T, --ttl <n>`: Time to Live bits
  - `-b, --background`: Run ping in background
  - `-4, --ipv4`: Force IPv4
  - `-6, --ipv6`: Force IPv6
- **Description**: Sends ICMP ping requests to a network host.

### Command: `lte_at`
- **Help**: Send an AT command to LTE modem
- **Arguments**:
  - `<command>` (required): AT command
  - `<timeout>` (optional): Timeout in ms
- **Description**: Sends raw AT commands to the LTE modem.

### Command: `lte_connect`
- **Help**: Connect LTE modem
- **Arguments**: None
- **Description**: Initiates LTE modem connection.

### Command: `lte_disconnect`
- **Help**: Disconnect LTE modem
- **Arguments**: None
- **Description**: Disconnects the LTE modem.

### Command: `lte_clear_sim`
- **Help**: Clear SIM
- **Arguments**: None
- **Description**: Clears SIM card configuration.

### Command: `lte_info`
- **Help**: Show LTE modem info
- **Arguments**: None
- **Description**: Displays LTE modem information and status.

### Command: `lte_power`
- **Help**: Control LTE modem power
- **Arguments**: None
- **Description**: Controls LTE modem power on/off.

### Command: `lte_restart`
- **Help**: Restart LTE modem
- **Arguments**: None
- **Description**: Restarts the LTE modem.

### Command: `lte_get_dcd`
- **Help**: Get DCD (Data Carrier Detect) status
- **Arguments**: None
- **Description**: Gets the Data Carrier Detect status from LTE modem.

### Command: `lte_get_sim_type`
- **Help**: Get SIM type
- **Arguments**: None
- **Description**: Queries the SIM card type from the modem.

### Command: `lte_band`
- **Help**: Get/Set LTE band
- **Arguments**: None
- **Description**: Manages LTE band configuration.

### Command: `lte_sim_iccid`
- **Help**: Get SIM ICCID
- **Arguments**: None
- **Description**: Displays the SIM card ICCID number.

### Command: `wifi_info`
- **Help**: Get WiFi information
- **Arguments**: None
- **Description**: Displays current WiFi connection information (RSSI).

### Command: `wifi_scan`
- **Help**: Scan available WiFi networks
- **Arguments**: None
- **Description**: Scans and lists available WiFi networks.

### Command: `ap_clients`
- **Help**: List WiFi AP clients
- **Arguments**: None
- **Description**: Lists connected WiFi access point clients.

---

## Parameters Commands

### Command: `param_list`
- **Help**: List params
- **Arguments**:
  - `-d, --defaults`: Show default values
  - `-n, --non-defaults`: Show non-default values only
- **Description**: Lists all parameters with their current or default values.

### Command: `param_get`
- **Help**: Get param value
- **Arguments**:
  - `<key>` (required): Parameter key
- **Description**: Retrieves the value of a specific parameter.

### Command: `param_set`
- **Help**: Set key-value pair in selected namespace
- **Arguments**:
  - `<key>` (required): Parameter key
  - `<value>` (required): One or more values
- **Description**: Sets parameter values.

### Command: `param_reset`
- **Help**: Reset param to default values
- **Arguments**:
  - `<param_id>` (optional): Specific parameter ID to reset
- **Description**: Resets parameters to their default values.

### Command: `param_store`
- **Help**: Store all param changes to flash
- **Arguments**:
  - `<param_id>` (optional): Specific parameter ID
  - `<storage>` (optional): Storage name
  - `-c, --clean`: Clean storage if values contain defaults
- **Description**: Persists parameter changes to flash storage.

### Command: `param_load`
- **Help**: Load all param changes from flash
- **Arguments**:
  - `<storage>` (optional): Storage name to load from
- **Description**: Loads parameters from flash storage.

---

## Reboot Commands

### Command: `reset`
- **Help**: Software reboot of the chip
- **Arguments**:
  - `<reset_delay_ms>` (optional): Delay in milliseconds before reboot, 0 to abort
- **Description**: Performs a software reboot with optional delay.

### Command: `factory_reset`
- **Help**: Factory reset all the parameters
- **Arguments**:
  - `-n, --no-restart`: Do not restart after factory reset
- **Description**: Resets all parameters to factory defaults with optional restart.

---

## Debug LED Commands

### Command: `debug_led_set`
- **Help**: Set pwm value for debug led
- **Arguments**:
  - `<index>` (required): Index of debug LED
  - `<value>` (required): PWM value (0.0 - 1.0)
- **Description**: Sets the PWM duty cycle for a debug LED.

---

## EMS (Energy Management System) Commands

### Command: `ems`
- **Help**: Show ems information
- **Arguments**: `[all|bms_data|bms_info|client|ems_config|ems_control|ems_data|ems_energy_aggregate|ems_info|ems_prediction|error_cnt|inv_info|inv_config|sensor]`
- **Description**: Displays various EMS-related information and statistics.

### Command: `sched_add`
- **Help**: Add schedule to queue
- **Arguments**: (extensive - see source for full list)
  - `<type>` (required): Schedule type (0=idle, 1=inv-charge, 2=inv-discharge, etc.)
  - `--from <from>`: Start time (YYYY-MM-DDTHH:mm:ss)
  - `--to <to>`: End time
  - And many more power and frequency reserve parameters...
- **Description**: Adds a new schedule to the EMS schedule queue.

### Command: `sched_set`
- **Help**: Set/replace schedule
- **Arguments**: (same as sched_add, but clears existing schedules)
- **Description**: Replaces all schedules with a new one.

### Command: `sched_del`
- **Help**: Delete schedule by ID
- **Arguments**:
  - `<id>` (required): Schedule ID to delete
- **Description**: Deletes a specific schedule by ID.

### Command: `sched_clear`
- **Help**: Clear all schedules
- **Arguments**: None
- **Description**: Clears all schedules from the queue.

### Command: `sched_list`
- **Help**: List all schedules
- **Arguments**: None
- **Description**: Displays all active schedules with their parameters.

### Command: `ecu_ffr_rearm`
- **Help**: Rearm FFR (Fast Frequency Response)
- **Arguments**: None
- **Description**: Sends signal to rearm FFR functionality.

### Command: `ecu_ctx_reset`
- **Help**: Reset ECU context
- **Arguments**: None
- **Description**: Resets the ECU control context.

### Command: `energy`
- **Help**: List or modify energy counters
- **Arguments**:
  - `-n, --name <name>`: Energy aggregate name
  - `-i, --imported <kWh>`: Set imported energy in kWh
  - `-e, --exported <kWh>`: Set exported energy in kWh
  - `-s, --store`: Store values to flash immediately
- **Description**: Displays and modifies energy counter values.

### Command: `ecu_network`
- **Help**: Show network information
- **Arguments**: None
- **Description**: Displays ECU network information.

### Command: `ecu_network_reset`
- **Help**: Reset network
- **Arguments**: None
- **Description**: Resets the network subsystem.

---

## EMS CLI Commands

### Command: `modbus_read`
- **Help**: Read Modbus register
- **Arguments**: (Check source for details)
- **Description**: Reads Modbus registers from EMS devices.

### Command: `modbus_write`
- **Help**: Write Modbus register
- **Arguments**: (Check source for details)
- **Description**: Writes to Modbus registers on EMS devices.

### Command: `ems_reset`
- **Help**: Reset EMS
- **Arguments**: None
- **Description**: Resets the EMS system.

### Command: `ems_set_config`
- **Help**: Set EMS configuration
- **Arguments**: (Check source for details)
- **Description**: Configures EMS grid codes and control parameters.

### Command: `inverter_set_config`
- **Help**: Set inverter configuration
- **Arguments**: (Check source for details)
- **Description**: Configures inverter frequency response parameters.

---

## ECU Sense Assignment Commands

### Command: `ecu_sense_assign`
- **Help**: Assign a sensor node to a function
- **Arguments**: (Check source)
- **Description**: Assigns a sensor node to a specific measurement function.

### Command: `ecu_sense_clear`
- **Help**: Clear sensor assignments (all or specific node)
- **Arguments**: (Check source)
- **Description**: Clears sensor assignments globally or for a specific node.

### Command: `ecu_sense_list`
- **Help**: List all sensor assignments
- **Arguments**: None
- **Description**: Lists all current sensor assignments and their functions.

---

## Provisioning Commands

### Command: `provision`
- **Help**: provision bridge
- **Arguments**:
  - `<url>` (optional): Provisioning URL
  - `<auth>` (optional): Provisioning HTTP authentication
  - `--no-device`: Do not use device cert to provision
- **Description**: Provisions the device via provisioning bridge.

---

## Power Commands

### Command: `power`
- **Help**: Power management
- **Arguments**:
  - `i`: Show power info
  - `c`: Clear power info counters
- **Description**: Displays power consumption info (ECU current, supercap voltage, charge percent).

---

## System Mode Commands

### Command: `system_mode`
- **Help**: Show current system mode
- **Arguments**: None
- **Description**: Displays the current system operating mode.

### Command: `system_mode_force`
- **Help**: Set system mode
- **Arguments**:
  - `<mode>` (required): Mode (0=init, 1=production, 2=operational, 3=standalone, 4=service)
- **Description**: Forces the system into a specific operating mode.

---

## Status and Metrics Commands

### Command: `status`
- **Help**: Show system status
- **Arguments**: None
- **Description**: Displays status of all ECU subsystems with color-coded health indicators.

### Command: `metrics_send`
- **Help**: Force sending metrics
- **Arguments**: None
- **Description**: Immediately sends metrics via MQTT.

### Command: `log_mqtt`
- **Help**: Stream logs via mqtt
- **Arguments**:
  - `<interval_ms>` (required): Log interval in ms (0 to stop)
  - `<duration_s>` (required): Duration in seconds (0 = never stop)
- **Description**: Streams system logs via MQTT protocol.

---

## Monitoring Commands

### Command: `pulse_monitor_status`
- **Help**: Show pulse monitor status and timestamps
- **Arguments**: None
- **Description**: Displays pulse monitor status with timestamp information.

---

## Worker Task Commands

### Command: `worker`
- **Help**: Manage worker tasks
- **Arguments**:
  - `-i, --id <id>`: Worker task ID
  - `-n, --name <name>`: Worker task name
  - `--reset`: Schedule immediate run
  - `--period <ms>`: Set execution period
  - `--enable`: Enable worker
  - `--disable`: Disable worker
  - `--now`: Run immediately
  - `--once`: Run once
- **Description**: Lists and controls background worker tasks.

---

## CLI Commands

### Command: `smart`
- **Help**: Smart terminal mode
- **Arguments**: None
- **Description**: Controls terminal smart mode detection.

### Command: `clear`
- **Help**: Clear screen
- **Arguments**: None
- **Description**: Clears the terminal screen.
