# Unitree Go2 EDU + Orin quickstart

Quickly gain insight in Go2 EDU + expansion dock:
- interfaces
- how to connect
- how to backup

![Go2 EDU top view](img/orin_dock_install_small.png)

[Unitree Payload documentation](https://support.unitree.com/home/en/developer/Payload)


## System architecture
![Unitree Go2 system architecture](img/architecture_overview.jpg)

[Unitree Architecture Description](https://support.unitree.com/home/en/developer/Architecture%20Description)

| Interface hardware | Protocol / data | Intended use | How to connect |
|---|---|---|---|
| Bluetooth Low Energy | BLE | Initial pairing, app setup and network configuration | Pair the Go2 in the Unitree Go App |
| Wi-Fi module | Dual-band Wi-Fi 6 (`802.11ax`), TCP/IP, WebRTC | Local control, FPV video and app/PC communication | Join a router through the app or use the Go2 access point |
| Cellular | 4G LTE/eSIM, MQTT, WebRTC, HTTP | Remote telemetry, monitoring and OTA updates | Configure cellular service in the app; availability depends on region and authorization |
| Ethernet | RJ45 or supported dock Ethernet connector; DDS/TCP/IP/GST | Reliable SDK, ROS 2, video and computer-to-robot communication | Connect the PC or Orin and use the same `192.168.123.0/24` subnet |
| Ultra-Wideband | UWB ranging and positioning | Follow-me mode and tracking-remote positioning | Power and pair the supported UWB remote/tag |
| Internal serial buses | UART, CAN and SPI | Communication between motors, sensors and internal controllers | Internal wiring; not a normal user-facing interface |

## Hardware & interfaces

![Orin expansion dock interfaces](img/orin_dock_interface_1814x396.png)

[Unitree Payload documentation](https://support.unitree.com/home/en/developer/Payload)

**Ports frontside**: GH1.25-4PIN Gigabit ethernet, M8 plug, power pins, RJ45 (go2 main board), USB-C (optional depth camera)

**Ports backside**: USB-A, Power supply pins, RJ45, USB-C

## Development

### Monitor

Connecting a display screen requires a **USB-C-to-HDMI** adapter.

![Connection a display screen](img/orin_monitor_usb_hdmi.png)
[Unitree Expansion dock config](https://support.unitree.com/home/en/developer/module_update)
### Connection

Expansion dock (orin) connects to  Go2 by power cable (28.8 V DC) and ethernet.

| Hardware Component | Static IP Address | Subnet Mask | Purpose / Notes |
|---|---|---|---|
| Expansion Dock PC | **`192.168.123.18`** | `255.255.255.0` | SSH and run ROS 2 code. |
| Motion Control PC | `192.168.123.161` | `255.255.255.0` | Low-level controller (restricted access). |
| Development PC | `192.168.123.222` | `255.255.255.0` | Suggested static IP|

```ssh unitree@192.168.123.18```
default password: 123



### Interfaces

| Interface | Description |
|---|---|
| DDS | **Data Distribution Service**: the Go2's publish/subscribe communication layer for sending commands and receiving robot data. Supported from C++ and Python through the **Unitree SDK**. |
| ROS 2 | Robotics middleware interface for using Go2 data and services as ROS 2 topics, services and messages. ROS 2 normally uses DDS underneath. |
| GST | **GStreamer** interface used only for transmitting camera images/video; it is not a robot-control interface. |

## Orin filesystem

Unitree does not publish a complete inventory of every factory image. A typical installation looks like this:

```text
/
├── home/
│   └── unitree/          # User code and optional ROS workspaces
├── opt/
│   ├── ros/foxy/         # ROS 2 Foxy, when installed
│   └── unitree_robotics/ # Possible SDK2 installation location
├── usr/local/
│   └── cuda/             # NVIDIA CUDA
├── etc/
│   ├── systemd/          # System services
│   └── NetworkManager/   # Network configuration, including static Ethernet
├── var/log/              # System and service logs
└── boot/                 # Kernel, device tree and Jetson boot configuration
```

### Inspect the actual installation

Connect through SSH and run the following commands to obtain the exact OS version, Jetson version, installed packages, directories and enabled services of your dock:

```bash
ssh unitree@192.168.123.18

cat /etc/os-release
uname -a
cat /etc/nv_tegra_release
dpkg-query -W | sort
find /home/unitree -maxdepth 2 -type d | sort
ls -ld /opt/ros/* /opt/unitree_robotics /usr/local/cuda* 2>/dev/null
systemctl list-unit-files --state=enabled
systemctl --failed
```

## Orin backup

See [Orin backup](ORIN_BACKUP.md) for requirements and the complete backup procedure.
