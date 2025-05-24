# Linux or Unix in general

## Check if you are virtualized

1. You might be able to get and idea by looking around under /sys. For example /sys/class/dmi/id/sys_vendor has a value of VMware, Inc..
2. Linux adds the hypervisor flag to /proc/cpuinfo if the kernel detects running on some sort of a hypervisor.
3. dmesg | grep -i hypervisor
