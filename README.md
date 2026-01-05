# ParallelPing User Manual

Welcome to **ParallelPing** — a lightweight portable Windows desktop application for monitoring network latency across multiple hosts in real-time. This manual covers installation, basic usage, advanced technical features, and troubleshooting.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Main Interface Overview](#main-interface-overview)
3. [Managing Targets](#managing-targets)
4. [Monitoring & Visualization](#monitoring--visualization)
5. [Advanced Features](#advanced-features)
6. [Technical Architecture](#technical-architecture)
7. [Keyboard Shortcuts](#keyboard-shortcuts)
8. [Tips & Troubleshooting](#tips--troubleshooting)

---

## Getting Started

### System Requirements
- **Windows 7 or later** (Windows 10/11 recommended)
- **No administrator privileges required**

### Installation/Usage

#### Portable (No Install Needed)
- Download the **portable ZIP** (ParallelPing.exe + supporting files)
- Extract anywhere (USB drive, server jump host, or local folder)
- Run `ParallelPing.exe` directly — **no admin rights or installer required**

> Ideal for locked-down servers/jumphosts where software installation is restricted.



### First Launch
On first launch, ParallelPing loads **5 default targets** for quick testing:
- Google DNS (8.8.8.8)
- Cloudflare (1.1.1.1)
- OpenDNS (208.67.222.222)
- google.com
- github.com

These targets immediately start pinging every second. You can add, modify, or remove them as needed.

![First Launch Screenshot](screenshots/first_launch.png)
*Figure 1: ParallelPing's default interface showing 5 pre-configured targets*

---

## Main Interface Overview

![Main Interface Screenshot](screenshots/main_interface.png)
*Figure 2: Main interface with target list (left) and real-time latency graph (right)*

### Window Layout

```
┌──────────────────────────────────────────────────┐
│  ParallelPing - Toolbar                          │
├──────────────────────┬──────────────────────────┤
│                      │                          │
│   Target List        │    Live Latency Graph   │
│   (Table)            │    (PyQtGraph Plot)     │
│                      │                          │
│  • Address           │  • Hover for tooltip    │
│  • Hostname          │  • Shows exact values   │
│  • Current Latency   │  • Real-time update     │
│  • Statistics        │                          │
│                      │                          │
└──────────────────────┴──────────────────────────┘
```

### Toolbar Buttons

![Toolbar Screenshot](screenshots/toolbar.png)
*Figure 3: Main toolbar with all control buttons*

| Button | Function | Shortcut |
|--------|----------|----------|
| **Add Target** | Opens dialog to add a new ping target | Ctrl+A |
| **Remove** | Deletes selected target(s) from list | Delete |
| **Start** | Begins pinging all active targets | F5 |
| **Stop** | Pauses all pings | F6 |
| **Save** | Saves current target list to file | Ctrl+S |
| **Load** | Opens saved target list from file | Ctrl+O |
| **Export** | Exports ping history and stats to CSV/Excel | Ctrl+E |
| **Compare** | Overlay multiple targets on one graph | Ctrl+M |

---

## Managing Targets

![Add Target Dialog](screenshots/add_target_dialog.png)
*Figure 4: Add Target dialog showing all configuration options*

### Adding a Target

1. Click **Add Target** (or press Ctrl+A)
2. Enter the target information:
   - **Address**: IP address (e.g., `192.168.1.1`) or hostname (e.g., `google.com`)
   - **Label**: Optional friendly name (e.g., "Office Server")
   - **Ping Type**: Choose between:
     - **ICMP**: Standard ping (blocked by some firewalls)
     - **TCP**: Connect to a specific port (useful for firewall-blocked targets)
   - **Port**: For TCP pings, specify the port (default: 80)
   - **Packet Size**: ICMP payload size in bytes (default: 32)
   - **Group**: Organize targets by category (e.g., "Production", "Testing")
3. Click **OK**

The target immediately starts pinging and appears in the table.

### Modifying a Target

1. **Double-click** a row in the table to edit it
2. Update any field (address, label, ping type, etc.)
3. Click **OK** to apply changes

### Removing Targets

1. Select one or more targets in the table
2. Press **Delete** or click **Remove**
3. Confirm the removal

### Pausing/Resuming Individual Targets

Targets can be temporarily paused without removal:
- **Right-click** a target → **Pause** (or resume)
- Paused targets show a pause icon and do not consume ping requests
- Statistics continue to display historical data

### Organizing with Groups

Targets can be organized into groups for easier management:
1. Add targets with group names (e.g., "Production", "Testing")
2. Groups appear as visual separators in the table
3. Right-click a group to **Pause Group** or **Resume Group** at once

---

## Monitoring & Visualization

### Target List Table

The left side displays all targets with real-time statistics:

| Column | Meaning |
|--------|---------|
| **Target** | Hostname or address |
| **Current** | Latest latency in milliseconds (or "—" if no response) |
| **Min** | Minimum latency in the current time window |
| **Avg** | Average latency |
| **Max** | Maximum latency |
| **Loss %** | Packet loss percentage (0–100%) |
| **Jitter** | Latency variation; lower is more stable |
| **Status** | Connection status indicator (✓ = online, ✗ = offline) |

**Sorting**: Click any column header to sort ascending/descending.

**Color Coding**: Targets with high packet loss or latency appear highlighted in red.

### Live Latency Graph

![Live Graph with Tooltip](screenshots/live_graph_tooltip.png)
*Figure 5: Real-time latency graph showing multiple targets with hover tooltip*

The right side displays a **real-time plot** of latency over time:

- **X-axis**: Time (samples taken at 1-second intervals)
- **Y-axis**: Latency in milliseconds
- **Each target**: Plotted as a separate colored line
- **Scrolling**: Graph shows the rolling 5-10 minute window (configurable)

#### Hover Tooltip
Move your mouse over the graph to see:
- **Exact timestamp** of the data point
- **Latency value** for the target at that moment
- Crosshair guides to help identify data points

### Interpreting Results

| Condition | Meaning | Action |
|-----------|---------|--------|
| **Steady low latency** | Network is healthy | Monitor for changes |
| **Increasing latency** | Network congestion | Check if network load is high |
| **Packet loss > 0%** | Unreliable connection | Investigate network issues or target availability |
| **High jitter** | Unstable connection | May indicate congestion or network problems |
| **No response** | Target offline or blocking ICMP | Try TCP ping or verify target is reachable |

---

## Advanced Features

### Compare Multiple Targets

![Compare Window](screenshots/compare_window.png)
*Figure 6: Compare dialog overlaying multiple targets for side-by-side analysis*

Compare multiple targets on a single overlay graph to identify differences:

1. Select multiple targets in the table (Ctrl+Click or Shift+Click)
2. Press **Ctrl+M** or click **Compare**
3. A new window opens with all selected targets plotted together
4. Use zoom/pan to examine specific time ranges

**Use cases:**
- Compare latency between different ISP routes
- Verify if service degradation affects multiple targets equally
- Identify timing patterns in network performance

### Traceroute Diagnostics

![Traceroute Dialog](screenshots/traceroute_dialog.png)
*Figure 7: Traceroute results showing network path and hop latencies*

Trace the network path to a target to identify bottlenecks:

1. **Right-click** a target in the table
2. Select **Run Traceroute**
3. A dialog displays each hop with:
   - **Hop Number**: Sequence (1, 2, 3, etc.)
   - **Address**: IP or hostname of the router
   - **Latency**: Time to reach that hop
4. **Actions**:
   - **Copy**: Copy results to clipboard (as tab-separated text)
   - **Save**: Export to CSV or text file
   - **Close**: Dismiss the dialog

**Interpreting Traceroute:**
- Each line represents a router hop to the target
- Latency should generally increase with each hop
- `* * *` means the hop did not respond
- Look for sudden latency spikes to identify problem routers

### Export Data

![Export Dialog](screenshots/export_dialog.png)
*Figure 8: Export dialog with format and data selection options*

Export ping history and statistics for analysis in Excel, spreadsheet, or data analysis tools:

1. Click **Export** (or press Ctrl+E)
2. Choose export format:
   - **CSV**: Text format, opens in Excel or text editors
   - **Excel (.xlsx)**: Formatted spreadsheet with charts
3. Select **which data to include**:
   - Ping history (all samples)
   - Summary statistics
   - Per-target details
4. Choose save location and filename
5. Open the file to analyze results

**CSV Format Example:**
```
Timestamp,Target,Latency (ms),Status
2025-01-05 10:00:01,8.8.8.8,25.3,Success
2025-01-05 10:00:02,8.8.8.8,24.8,Success
2025-01-05 10:00:03,8.8.8.8,None,Loss
```

### Save & Load Target Lists

#### Saving
1. Configure your targets as desired
2. Click **Save** (or press Ctrl+S)
3. Choose a filename and location
4. File is saved in JSON format (human-readable)

#### Loading
1. Click **Load** (or press Ctrl+O)
2. Select a previously saved target list
3. Current targets are replaced with the loaded list
4. Pinging restarts automatically

**Auto-Load on Startup:**
If a file named `target-list.json` exists in the ParallelPing directory, it will be automatically loaded when the app starts.

### Customizing Targets

#### Ping Type: ICMP vs. TCP

**ICMP Ping** (Default)
- ✓ Fast and lightweight
- ✓ Works with most networks
- ✗ Blocked by some firewalls
- **Use for**: General network monitoring, routers, cloud infrastructure

**TCP Ping**
- ✓ Works through many firewalls
- ✓ Can test specific services (port 443 for HTTPS, 3306 for MySQL, etc.)
- ✗ May show longer latency if service is slow
- **Use for**: Testing service availability, firewall-restricted targets

#### Custom Packet Size
- **Default**: 32 bytes (standard ping)
- **Increase to**: Detect MTU (Maximum Transmission Unit) issues
  - Try 1500 bytes; if it fails, the link may have MTU problems
- **Use for**: Diagnosing fragmentation and network configuration issues

---

## Technical Architecture

### System Design Overview

ParallelPing is built on a **multi-threaded architecture** using PySide6 (Qt6) for the UI framework and PyQtGraph for real-time visualization. The application separates concerns into distinct layers:

![Architecture Diagram](screenshots/architecture_diagram.png)
*Figure 9: Application architecture showing data flow between components*

#### Core Components

1. **Data Model Layer** (`core/models.py`)
   - `Target` dataclass: Stores target configuration and ping results
   - Uses Python's `collections.deque` for fixed-size rolling history (memory-efficient)
   - History size: Configurable maximum samples (default: 600 samples = 10 minutes at 1-second intervals)
   - Statistics computed on-demand from windowed history using NumPy aggregations

2. **Networking Layer** (`core/ping_worker.py`)
   - **ICMP Implementation**: Spawns Windows `ping.exe` subprocess with localized regex parsing
   - **TCP Implementation**: Raw Python socket with `connect_ex()` timing measurement
   - **DNS Resolution**: `socket.gethostbyaddr()` for reverse lookups (cached)
   - Timeout handling: 1000ms default (configurable per target)

3. **UI Layer** (`ui/main_window.py`, `ui/dialogs.py`)
   - PySide6 QMainWindow with split view (QSplitter)
   - PyQtGraph `ScrollablePlotWidget` for GPU-accelerated real-time plotting
   - Custom item delegates for color-coded table cells
   - Modal dialogs for traceroute, compare, and export operations

4. **Persistence Layer** (`core/storage.py`, `core/settings.py`)
   - Target lists: JSON format (human-readable, version-controlled)
   - UI state: QSettings stored in Windows Registry (`HKEY_CURRENT_USER`)
   - Auto-save on exit; auto-load `target-list.json` on startup if present

### Threading Model

ParallelPing uses **Qt's QThreadPool** for concurrent ping execution without blocking the UI:

```
Main Thread (UI)
    ↓
QThreadPool (Worker Threads)
    ↓
PingTask (QRunnable) × N targets
    ↓ (Signal/Slot)
MainWindow.on_ping_result()
    ↓
Update Target.history (deque)
    ↓
Refresh Table & Graph
```

**Key Design Decisions:**
- Each target spawns a `PingTask` (QRunnable) every 1 second
- Workers emit `result_signal(target_id, latency, timestamp)` upon completion
- **Thread Safety**: All Target updates occur in the main thread via Qt's signal/slot mechanism (no locks needed)
- QThreadPool size: Auto-configured based on CPU cores (typically 4-16 workers)

### Data Structures & Memory Management

#### Target History (Rolling Buffer)
```python
from collections import deque

class Target:
    history: Deque[Optional[float]] = deque(maxlen=600)  # Fixed size
    
    def push(self, latency: Optional[float], timestamp: float):
        self.history.append((latency, timestamp))
        # Oldest sample auto-dropped when maxlen reached
```

**Benefits:**
- Constant O(1) push/pop operations
- Bounded memory: 600 samples × 16 bytes ≈ 9.6 KB per target
- 100 targets × 9.6 KB = 960 KB total history (negligible)

#### Statistics Computation (Windowed)
Statistics are computed **on-demand** from the rolling history:
```python
def stats(self, window_secs: int = 300) -> Dict[str, float]:
    # Filter samples within time window
    cutoff = time.time() - window_secs
    windowed = [(lat, ts) for lat, ts in self.history if ts >= cutoff]
    
    # Compute aggregates using NumPy
    latencies = [lat for lat, _ in windowed if lat is not None]
    return {
        "min": np.min(latencies),
        "max": np.max(latencies),
        "avg": np.mean(latencies),
        "jitter": np.std(latencies),  # Standard deviation
        "loss_pct": (len(windowed) - len(latencies)) / len(windowed) * 100
    }
```

### Ping Implementation Details

#### ICMP Ping (Windows ping.exe)
```python
import subprocess
import re

# Execute Windows ping command
result = subprocess.run(
    ["ping", "-n", "1", "-w", "1000", "-l", "32", target],
    capture_output=True,
    text=True,
    timeout=2.0
)

# Parse output with localized regex
TIME_RE = re.compile(r"[Tt]ime[<=](\d+)", re.IGNORECASE)
match = TIME_RE.search(result.stdout)
latency_ms = int(match.group(1)) if match else None
```

**Localization Handling:**
- Regex matches both `time=` and `time<` (for <1ms responses)
- Case-insensitive to handle different Windows languages
- Falls back to None on parse failure (treated as packet loss)

#### TCP Ping (Socket Connect)
```python
import socket
import time

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.settimeout(1.0)

start = time.perf_counter()
result = sock.connect_ex((target, port))  # Non-blocking
end = time.perf_counter()

latency_ms = (end - start) * 1000 if result == 0 else None
sock.close()
```

**Use Cases:**
- Firewall-blocked ICMP environments
- Testing specific service availability (HTTP=80, HTTPS=443, MySQL=3306)
- Latency includes TCP handshake overhead (~2-3ms additional)

### Real-Time Visualization

PyQtGraph provides GPU-accelerated plotting with minimal CPU overhead:

```python
from pyqtgraph import PlotWidget, mkPen

# Add target to plot
curve = self.plot_widget.plot(
    x=timestamps,
    y=latencies,
    pen=mkPen(color=target.color, width=2),
    name=target.label
)

# Update on new data (incremental)
curve.setData(x=new_timestamps, y=new_latencies)
```

**Performance Optimizations:**
- Downsampling: For >1000 samples, plot every Nth point (configurable)
- ViewBox auto-scaling disabled during updates (re-enabled after)
- OpenGL acceleration available (requires `pip install pyopengl`)

### File Format Specifications

#### Target List JSON (`target-list.json`)
```json
{
  "version": "1.0",
  "targets": [
    {
      "address": "8.8.8.8",
      "label": "Google DNS",
      "ping_type": "ICMP",
      "port": 0,
      "packet_size": 32,
      "group": "Public DNS",
      "enabled": true
    }
  ]
}
```

#### Export CSV Format
```csv
Timestamp,Target,Label,Latency (ms),Status,Packet Loss (%),Jitter (ms)
2026-01-05 10:00:01.234,8.8.8.8,Google DNS,25.3,Success,0.0,1.2
2026-01-05 10:00:02.456,8.8.8.8,Google DNS,24.8,Success,0.0,1.1
2026-01-05 10:00:03.678,8.8.8.8,Google DNS,,Loss,100.0,0.0
```

### Performance Characteristics

| Metric | Value | Notes |
|--------|-------|-------|
| **Ping Interval** | 1000 ms | Configurable (500-5000 ms) |
| **UI Refresh Rate** | 60 FPS | Qt event loop drives updates |
| **Max Recommended Targets** | 100 | Limited by thread pool size |
| **Memory per Target** | ~10 KB | Fixed-size history deque |
| **Network Bandwidth** | ~56 bytes/ping | ICMP packet size + overhead |
| **CPU Usage (idle)** | <1% | Event-driven architecture |
| **CPU Usage (100 targets)** | 3-5% | Mostly subprocess overhead |

### Extensibility Points

For developers looking to extend ParallelPing:

1. **Custom Ping Types**: Add new ping methods to `ping_worker.py` (e.g., UDP, HTTP GET)
2. **Additional Statistics**: Extend `Target.stats()` with new metrics (e.g., 95th percentile)
3. **Export Formats**: Add handlers to `ExportDialog` (e.g., JSON, InfluxDB line protocol)
4. **Alerting**: Implement threshold-based notifications via Qt signals
5. **Plugins**: Use Qt's plugin system for custom visualizations or analysis tools

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| **Ctrl+A** | Add new target |
| **Delete** | Remove selected target(s) |
| **F5** | Start pinging |
| **F6** | Stop pinging |
| **Ctrl+S** | Save target list |
| **Ctrl+O** | Open/load target list |
| **Ctrl+E** | Export to CSV/Excel |
| **Ctrl+M** | Compare selected targets |
| **Ctrl+Q** | Quit application |
| **Escape** | Close dialogs |

---

## Tips & Troubleshooting

### Common Issues

#### "No response" for all targets
- **Check**: Is pinging enabled? Click **Start** (F5)
- **Check**: Is internet connection active? Test by opening a browser
- **Solution**: Click **Start** to begin pinging again

#### ICMP Ping Shows No Response but TCP Succeeds
- **Cause**: Firewall or network policy is blocking ICMP
- **Solution**: Use TCP ping instead (change Ping Type to "TCP")
- **Alternative**: Ask IT to allow ICMP, or use a different port

#### High Jitter or Packet Loss
- **Cause**: Network congestion, Wi-Fi interference, or a problematic ISP route
- **Check**: Use **Compare** mode to see if other targets are affected
- **Check**: Use **Traceroute** to identify which hop is problematic
- **Action**: Check your router temperature, Wi-Fi signal strength, or contact your ISP

#### Graph Scrolls Slowly or App Feels Sluggish
- **Cause**: Too many targets or very long history window
- **Solution**: Reduce number of simultaneous targets, or close the compare window
- **Solution**: Right-click window → **Clear History** to start fresh

#### Can't Save or Load Target Lists
- **Check**: Ensure the folder is writable (not on a read-only network drive)
- **Check**: Disk space is available
- **Check**: File is not open in another program

### Performance Tips

1. **Limit active targets**: 20–30 targets usually runs smoothly; very large numbers (100+) may slow down the UI
2. **Use appropriate timeouts**: 1000 ms (1 second) is standard; higher values mean slower feedback but reduced network traffic
3. **Close unnecessary dialogs**: Keep Compare and Traceroute windows closed when not needed
4. **Reduce ping interval**: By default, targets are pinged every 1 second; this is optimal for most use cases
5. **Monitor on a dedicated monitor**: Running ParallelPing on a second display helps keep it visible during work

### Network Diagnostics with ParallelPing

**Identify ISP problems:**
1. Add 3–4 targets (local router, ISP gateway, public DNS, public service)
2. Check if loss/jitter affects all targets equally (entire link problem) or specific targets (service issue)
3. Use Traceroute to see which hop introduces latency

**Test VPN or Proxy:**
1. Add targets inside and outside your VPN
2. Compare latency increase
3. If VPN causes >50% latency increase, consider optimizing VPN configuration

**Monitor during data transfers:**
1. Keep ParallelPing running while uploading/downloading large files
2. Watch for latency spikes during transfer (indicates network saturation)
3. Use jitter to detect if connection is becoming unstable

---

## Getting Help

### Built-in Help
- **Hover over buttons**: Tooltips explain each feature
- **Right-click a target**: Context menu shows available actions
- **Double-click a column header**: Sorts results

### Reporting Issues
If you encounter a bug:
1. Note the exact steps to reproduce
2. Check your internet connection
3. Ensure you're using the latest version
4. Report with: target address, error message, and operating system version

### Keyboard Shortcut Reference
Press **Ctrl+?** (or check this manual) to view all available shortcuts at any time.

---

## FAQ

**Q: Will ParallelPing work on macOS or Linux?**
A: The current version is Windows-only due to reliance on the Windows `ping` command. Future versions may support other platforms.

**Q: How much network bandwidth does ParallelPing use?**
A: Negligible. A single ping is ~56 bytes, so 30 targets pinged every second uses only ~1.7 KB/s (about 1.5 MB/hour).

**Q: Can I ping 1000 targets?**
A: Technically yes, but UI performance will degrade. For monitoring many targets, consider exporting data and analyzing with Excel or a monitoring system like Grafana.

**Q: What's the difference between "Min" and "Current" latency?**
A: **Current** = latest single ping. **Min** = lowest seen in the current time window (usually the last 5–10 minutes).

**Q: Does ParallelPing run in the background?**
A: Not yet. The app window must be open to continue pinging. You can minimize it to the system tray (if enabled).

**Q: Can I export data to a database?**
A: You can export to CSV and import into Excel, or use a data analysis tool to process the CSV files.

---

**Thank you for using ParallelPing! We hope it helps you monitor your network effectively.**

For the latest version and updates, check the project repository or releases page.
