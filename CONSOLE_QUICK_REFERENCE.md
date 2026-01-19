# Console Commands Quick Reference

Quick lookup table for all console commands on Tibber ECU.

## Quick Command Index

| Command | Purpose | Common Args |
|---------|---------|-------------|
| `info` | System & SDK info | none |
| `reset_hard` | Hardware reset | none |
| `event_dump` | Event loop info | none |
| `echo` | Print string | `<string>` |
| `tasks` | Running tasks | none |
| `time` | Date/time/uptime | `[date\|uptime\|ticks]` |
| `history` | Command history | none |
| `free` | Memory info | none |
| `mem` | Memory dump/read | `dump/read/write <addr> [len]` |
| `heap_trace_start` | Start heap trace | none |
| `heap_trace_stop` | Stop heap trace | none |
| `heap_trace_dump` | Show heap trace | none |
| `dmesg` | System log buffer | none |
| `log_list` | List loggers | `[search]` |
| `log_set` | Set log level | `<tag> <0-5>` |
| `log_reset` | Reset log levels | `[level]` |
| `ls` | List files | `<path>` |
| `cat` | View/create file | `<path> [data]` |
| `sha1` | File hash | `<path>` |
| `rm` | Delete file | `<path>` |
| `format` | Format filesystem | none |
| `param_list` | List parameters | `[search]` |
| `param_get` | Get param value | `<name>` |
| `param_set` | Set param value | `<name> <val>` |
| `param_reset` | Reset to defaults | `[name]` |
| `param_store` | Save to flash | none |
| `param_load` | Load from flash | none |
| `coredump_crash` | Trigger crash | none |
| `coredump_upload` | Upload dump | `<url> [file] [-e]` |
| `coredump_erase` | Erase dump | none |
| `ems` | EMS status | none |
| `ems_reset` | Reset EMS | none |
| `modbus_read` | Read register | `<addr>` |
| `modbus_write` | Write register | `<addr> <val>` |
| `sched_add` | Add schedule | `<HH:MM> <mode>` |
| `sched_set` | Modify schedule | `<id> <param> <val>` |
| `sched_del` | Delete schedule | `<id>` |
| `sched_list` | Show schedules | none |
| `sched_clear` | Clear all | none |
| `ecu_ffr_rearm` | Rearm FFR | none |
| `ecu_ctx_reset` | Reset context | none |
| `ecu_network` | Network status | none |
| `ecu_network_reset` | Reset network | none |
| `ecu_sense_assign` | Assign sensor | `<node> <func> [-i]` |
| `ecu_sense_clear` | Clear sensors | `[node]` |
| `ecu_sense_list` | List sensors | none |
| `lte_info` | Modem status | none |
| `lte_power` | Modem power | `<0/1>` |
| `lte_restart` | Restart modem | none |
| `lte_band` | Change band | `<band>` |
| `lte_sim_iccid` | SIM ICCID | none |

## Log Levels Reference

| Level | Value | Description |
|-------|-------|-------------|
| NONE | 0 | No logging |
| ERROR | 1 | Errors only |
| WARN | 2 | Warnings and errors |
| INFO | 3 | General info (default) |
| DEBUG | 4 | Debug messages |
| VERBOSE | 5 | All messages |

**Usage**: `log_set <tag> <0-5>` or `log_set * 4` for all loggers

## Quick Recipes

### Debug Specific Subsystem
```bash
log_set mqtt 4      # Increase MQTT logging
log_set * 2         # Reduce other noise
dmesg               # Check messages
```

### Save Configuration
```bash
param_set param1 value1
param_set param2 value2
param_store         # Save to flash
```

### List & Monitor Everything
```bash
param_list          # Show all parameters
sched_list          # Show schedules
ecu_sense_list      # Show sensors
ems                 # Show EMS status
```

### Troubleshoot Network
```bash
lte_info            # Check modem
ecu_network         # Check transport
log_set * 4         # Enable debug
lte_restart         # Reboot modem
```

### Check System Health
```bash
info                # Basic info
free                # Memory
tasks               # Running tasks
time uptime         # Uptime
param_list          # All settings
```

### Factory Reset (Careful!)
```bash
param_reset         # Reset to defaults
format              # Erase filesystem (!)
param_store         # Save reset state
reset_hard          # Reboot
```

## Connection Settings

```
Endpoint: POST /console.json
Auth: HTTP Basic (admin:<password>)
Base URL: http://homevolt-<deviceid>.local

Example:
curl -u admin:<password> -X POST -d 'cmd=info' http://homevolt-<deviceid>.local/console.json
```

## Common Keyboard Shortcuts

| Key | Action |
|-----|--------|
| TAB | Auto-complete command |
| UP/DOWN | Command history |
| Ctrl+C | Interrupt command |
| Ctrl+U | Clear line |
| Ctrl+A | Beginning of line |
| Ctrl+E | End of line |

## Categories by Purpose

### System Diagnostics
`info` → `free` → `tasks` → `time` → `dmesg`

### Parameter Management
`param_list` → `param_get` → `param_set` → `param_store`

### Logging Control
`log_list` → `log_set` → `dmesg` → `log_reset`

### Energy Management
`ems` → `sched_list` → `sched_add` → `param_store`

### Network Management
`ecu_network` → `lte_info` → `lte_restart` → `dmesg`

### Sensor Configuration
`ecu_sense_list` → `ecu_sense_assign` → `param_store`

### Filesystem Operations
`ls` → `cat` → `sha1` → `rm` → `format`

### Memory Debugging
`free` → `mem` → `heap_trace_start` → `heap_trace_dump`

