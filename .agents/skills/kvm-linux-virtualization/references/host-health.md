# Host Health — KVM Virtualization

## Initial Checks

1. OS and kernel: `uname -a`.
2. KVM module: `lsmod | grep kvm`.
3. CPU virtualization: `grep -cE 'vmx|svm' /proc/cpuinfo`.
4. libvirt daemon: `systemctl status libvirtd` (or `virtqemud`).
5. QEMU version: `qemu-system-x86_64 --version`.
6. Active domains: `virsh list --all`.

## Resource Pressure

- CPU: `top`, `mpstat`, check host load average.
- Memory: `free -h`, check swap usage.
- Disk: `df -h`, check storage pool capacity.
- I/O: `iostat`, `iotop` for disk bottlenecks.

## Kernel Messages

- `dmesg | tail -50` for recent errors.
- Check for OOM killer events.
- Check for KVM or QEMU errors.
- Check for hardware errors (MCE, ECC).

## Time Synchronization

- Host time must be accurate — VMs inherit or sync from host.
- Check `chronyc tracking` or `timedatectl`.

## Evidence Needed

- Host OS (Ubuntu version? RHEL version?).
- Kernel version.
- libvirt/QEMU versions.
- Number of running VMs vs host resources.
