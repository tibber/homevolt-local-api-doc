# Console Commands Documentation - Quick Start

## 📖 What Is This?

Complete documentation of all console commands available on the Tibber ECU. These commands are used for system debugging, configuration, and troubleshooting via the HTTP console API.

## 🚀 Getting Started in 30 Seconds

### 1. Access the Console API
```bash
# Using curl (Linux/Mac/Windows)
curl -u admin:<password> -X POST \
  -H "Content-Type: application/json" \
  -d '{"cmd":"info"}' \
  http://homevolt-<deviceid>.local/console.json
```

### 2. Try Your First Command
```
> info                    # Shows system info
> time                    # Shows current time
> param_list              # Lists all parameters
```

### 3. Find What You Need
| Need | Go To |
|------|-------|
| Quick command lookup | `CONSOLE_QUICK_REFERENCE.md` |
| Detailed guide | `CONSOLE_COMMANDS_REFERENCE.md` |
| Full inventory | `CONSOLE_COMMANDS_INVENTORY.md` |

## 📚 Documentation Files

### Quick Reference (Start Here) ⭐
**[CONSOLE_QUICK_REFERENCE.md](CONSOLE_QUICK_REFERENCE.md)**
- 50+ commands in quick lookup table
- Log level reference
- Keyboard shortcuts
- Quick recipes
- ~7 KB, easy to scan

### Detailed Guide (Complete Reference)
**[CONSOLE_COMMANDS_REFERENCE.md](CONSOLE_COMMANDS_REFERENCE.md)**
- Every command fully documented
- Arguments and examples
- Common workflows
- Troubleshooting section
- ~31 KB, comprehensive

### Inventory (Reference)
**[CONSOLE_COMMANDS_INVENTORY.md](CONSOLE_COMMANDS_INVENTORY.md)**
- Complete list of 80+ commands
- Organized by category
- File locations
- ~30 KB, structured reference

### Task Completion
**[CONSOLE_TASK_COMPLETION.md](CONSOLE_TASK_COMPLETION.md)**
- What was created
- Statistics
- Verification checklist
- How to use the docs

### API Integration
**[API_DOCUMENTATION.yaml](API_DOCUMENTATION.yaml)** (Enhanced)
- Console commands in OpenAPI 3.0 format
- For use with code generators
- Complements REST API documentation

## 🎯 Common Tasks

### "I want to check system health"
```bash
> info              # Chip, firmware, uptime
> free              # Memory status
> tasks             # Running tasks
> time uptime       # System uptime
```
→ See: `CONSOLE_QUICK_REFERENCE.md` - System Diagnostics section

### "I want to see console commands for energy management"
```bash
> ems               # EMS status
> sched_list        # View schedules
> param_list battery # Battery parameters
```
→ See: `CONSOLE_QUICK_REFERENCE.md` - Energy Management

### "I want to configure parameters"
```bash
> param_list
> param_get battery_mode
> param_set battery_mode 2
> param_store       # Save to flash
```
→ See: `CONSOLE_COMMANDS_REFERENCE.md` - Configuration Management Workflow

### "I need to troubleshoot a problem"
```bash
> log_list          # See available loggers
> log_set * 4       # Set all to DEBUG
> dmesg             # View logs
```
→ See: `CONSOLE_COMMANDS_REFERENCE.md` - Troubleshooting section

### "I want to find all commands related to sensors"
```bash
> grep -i sensor CONSOLE_QUICK_REFERENCE.md
> grep -i "ecu_sense" CONSOLE_COMMANDS_REFERENCE.md
```

## 📋 Available Commands by Category

| Category | Commands | Examples |
|----------|----------|----------|
| **System Info** | 3 | info, reset_hard, event_dump |
| **Tasks** | 2 | echo, tasks |
| **Time** | 2 | time, history |
| **Memory** | 6 | free, mem, heap_trace_* |
| **Logging** | 4 | dmesg, log_list, log_set, log_reset |
| **Files** | 6 | ls, cat, sha1, rm, format |
| **Parameters** | 6 | param_list, param_get, param_set, ... |
| **Crash Dumps** | 3 | coredump_crash, coredump_upload, ... |
| **EMS** | 4 | ems, ems_reset, modbus_read, ... |
| **Schedules** | 5 | sched_add, sched_list, sched_del, ... |
| **ECU** | 4 | ecu_network, ecu_ctx_reset, ... |
| **Sensors** | 3 | ecu_sense_assign, ecu_sense_list, ... |
| **LTE** | 5 | lte_info, lte_power, lte_band, ... |

**Total**: 43+ commands

## 🔧 Connection Settings

```
Device: Tibber ECU
Access Method: HTTP API
Endpoint: POST /console.json
Authentication: HTTP Basic Auth
Base URL: http://homevolt-<deviceid>.local or https://homevolt-<deviceid>.local
```

### Tools You Can Use
- **Command Line**: `curl`
- **GUI Tools**: Postman, Insomnia
- **Programming**: Any HTTP client library
- **Browser**: JavaScript fetch/XMLHttpRequest

### Example API Calls
```bash
# Using curl
curl -u admin:<password> -X POST \
  -H "Content-Type: application/json" \
  -d '{"cmd":"info"}' \
  http://homevolt-<deviceid>.local/console.json

# Using curl with multiple commands
curl -u admin:<password> -X POST \
  -H "Content-Type: application/json" \
  -d '{"cmd":"param_list"}' \
  http://homevolt-<deviceid>.local/console.json
```

## ⌨️ Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **TAB** | Auto-complete command |
| **UP/DOWN** | Command history |
| **Ctrl+C** | Interrupt running command |
| **Ctrl+U** | Clear current line |
| **Ctrl+A** | Go to start of line |
| **Ctrl+E** | Go to end of line |
| **Ctrl+L** | Clear screen |

## 📖 How to Use This Documentation

### For Quick Lookups
1. Open `CONSOLE_QUICK_REFERENCE.md`
2. Use Ctrl+F to search for command name
3. See the command arguments and quick example

### For Learning a Subsystem
1. Find the category in `CONSOLE_COMMANDS_REFERENCE.md`
2. Read detailed descriptions and examples
3. Follow the workflow examples

### For System Diagnostics
1. Check the Troubleshooting section in `CONSOLE_COMMANDS_REFERENCE.md`
2. Or search for your issue keyword
3. See recommended commands to run

### For Understanding Structure
1. Read `CONSOLE_QUICK_REFERENCE.md` for quick lookup table
2. Understand file organization
3. Choose appropriate document for your task

## 💡 Common Patterns

### Check & Set Configuration
```bash
> param_list
> param_get setting_name
> param_set setting_name new_value
> param_store               # Don't forget this!
```

### Enable Debug Logging
```bash
> log_list                  # See available loggers
> log_set * 4               # Set all to DEBUG
> log_set mqtt 5            # MQTT to VERBOSE
> dmesg                     # View logs
```

### Monitor Energy Management
```bash
> ems
> sched_list
> param_list battery
> param_list charge
> param_list schedule
```

### Troubleshoot Connectivity
```bash
> ecu_network
> lte_info
> log_set * 4
> lte_restart
> dmesg
```

## ✨ Pro Tips

1. **Chain Commands**: Send multiple commands in sequence via API
2. **Save Output**: Redirect curl output to file: `curl ... > output.json`
3. **Pretty Print**: Use `jq` to format JSON: `curl ... | jq`
4. **Script Commands**: Create shell scripts for repeated tasks
5. **Error Handling**: Check HTTP status codes in responses

## 🔗 Related Documentation

### HTTP/REST API Documentation
- `API_DOCUMENTATION.md` - All 37 HTTP endpoints
- `API_QUICK_REFERENCE.md` - HTTP endpoint quick reference
- `Tibber_ECU_API.postman_collection.json` - For testing in Postman

### Integration Points
- Both console and HTTP API can access the same parameters
- Use console for low-level debugging
- Use HTTP API for automated/remote access
- Use Postman for testing endpoints

## ❓ FAQ

**Q: How do I connect to the console?**
A: Use the HTTP console API endpoint at POST /console.json. See Connection Settings above.

**Q: Are console commands documented anywhere else?**
A: Yes, in the source code:
- Command implementation functions: See `func` field in documentation
- File locations listed for each command
- Complete command reference available in documentation

**Q: Do I need the HTTP API if I have console access?**
A: No, but they complement each other:
- Console: Low-level debugging, direct access
- HTTP: Remote access, integration, automation

**Q: What's the difference between param_set and modbus_write?**
A: `param_set` modifies system parameters, `modbus_write` writes to Modbus registers

**Q: How do I save changes permanently?**
A: Run `param_store` after `param_set` to write to flash

**Q: Can I use console commands over SSH?**
A: Yes, the console is accessible remotely via the HTTP /console.json endpoint.

**Q: What happens if I run `format`?**
A: Erases the entire SPIFFS filesystem. No confirmation prompt!

## 📞 Support

1. Check `CONSOLE_QUICK_REFERENCE.md` for command syntax
2. See workflows in `CONSOLE_COMMANDS_REFERENCE.md`
3. Check Troubleshooting section for your issue
4. Review source code files listed in command documentation

---

**Last Updated**: December 18, 2024
**Documentation Version**: 1.0
**Commands Covered**: 50+
**Documentation Files**: 5

