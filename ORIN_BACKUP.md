# Orin backup

Unitree does not publish an official full-backup procedure for the Go2 Orin dock. The procedure below uses NVIDIA's Jetson backup tools and has not been validated by Unitree.

## When to make a backup

Create a full backup before:

- Updating or reflashing JetPack, L4T or Ubuntu.
- Changing the kernel, device tree, drivers or QSPI firmware.
- Replacing the Orin module or NVMe drive.
- Major ROS 2, CUDA or system-configuration changes.

For normal source-code changes, Git and a file backup are usually sufficient.

## What a complete backup contains

- `/home/unitree`: code, ROS workspaces, models and configuration.
- System configuration: NetworkManager, systemd, udev and shell settings.
- Software inventory: Ubuntu, L4T, ROS 2, CUDA, SDK and package versions.
- Docker volumes or the files needed to rebuild them.
- All NVMe partitions, not only the root filesystem.
- QSPI boot firmware.
- The matching Unitree BSP/recovery package and NVIDIA `Linux_for_Tegra` tools.
- A SHA-256 checksum.

> Copying `/home/unitree` or the live root filesystem is not a complete, independently restorable Jetson backup.

## Requirements

- An x86-64 Ubuntu host with at least 200 GB of free space.
- The NVIDIA Linux for Tegra/JetPack release matching the Orin installation.
- The matching Unitree BSP or recovery package.
- A USB-C data cable connected to the dock's full-function USB-C port.

Do not create or restore the backup with a different JetPack release or a generic Jetson BSP.

## Full backup procedure

### 1. Record the installed versions

Run on the Orin:

```bash
cat /etc/nv_tegra_release
cat /etc/os-release
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
```

Keep this output with the backup.

### 2. Enter Force Recovery Mode

Shut down the Go2, connect the dock to the Ubuntu host through USB-C and place the Orin in Force Recovery Mode. Follow Unitree's [Module Update instructions](https://support.unitree.com/home/en/developer/module_update) for the button/pin sequence.

Confirm detection on the Ubuntu host:

```bash
lsusb | grep -i NVIDIA
```

The result should contain `NVIDIA Corp. APX` and a USB ID starting with `0955:`.

### 3. Create the NVMe and QSPI backup

Run from the matching `Linux_for_Tegra` directory on the Ubuntu host:

```bash
cd Linux_for_Tegra
sudo ./tools/l4t_flash_prerequisites.sh
sudo systemctl stop udisks2.service
sudo systemctl start nfs-kernel-server.service
sudo ./tools/backup_restore/l4t_backup_restore.sh -b -e nvme0n1 jetson-orin-nano-devkit
```

The NVIDIA backup tool preserves the Jetson partitions, including NVMe and QSPI data. This is different from cloning only the `APP` root-filesystem partition.

### 4. Archive and verify the backup

Preserve the complete backup directory together with the exact BSP and `Linux_for_Tegra` release:

```bash
cd ..
sudo tar -czf go2-orin-backup.tar.gz Linux_for_Tegra/tools/backup_restore
sha256sum go2-orin-backup.tar.gz > go2-orin-backup.tar.gz.sha256
sha256sum --check go2-orin-backup.tar.gz.sha256
```

Keep at least two copies, with one stored away from the robot and development PC.

## Compatibility warning

The `-e nvme0n1` option is unavailable in some older JetPack 5.1.1 backup scripts. If the option is rejected, stop and obtain the matching recovery package or BSP from Unitree. Do not patch the scripts or flash a generic BSP without a tested recovery path.

## References

- [Unitree Module Update](https://support.unitree.com/home/en/developer/module_update)
- [NVIDIA: Backing Up and Restoring a Jetson Device](https://docs.nvidia.com/jetson/archives/r36.4/DeveloperGuide/SD/FlashingSupport.html#backing-up-and-restoring-a-jetson-device)
