# Lab 07 - Exploring SysFS

## Objective

Learn sysfs.

---

## Network Devices

```bash
ls /sys/class/net
```

---

## MAC Address

```bash
cat /sys/class/net/*/address
```

---

## Block Devices

```bash
ls /sys/block
```

---

## CPU Information

```bash
ls /sys/devices/system/cpu
```

---

## Kernel Modules

```bash
ls /sys/module | head
```

---

## Questions

1. What does sysfs expose?
2. Difference between procfs and sysfs?
3. What is a kernel object?
