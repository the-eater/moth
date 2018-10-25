# moth

Sticky sticky.

---

Moth helps with forcing some PCI slots to bind to certain kernel modules or drivers.

In my situation I have 2 NVIDIA Graphical cards in which 1 needs to be bound to `vfio-pci`, the other card should be bound to whatever the default is.

My `lspci` looks like the following:

```
01:00.0 VGA compatible controller: NVIDIA Corporation GP104 [GeForce GTX 1080] (rev a1)
01:00.1 Audio device: NVIDIA Corporation GP104 High Definition Audio Controller (rev a1)
03:00.0 VGA compatible controller: NVIDIA Corporation GP104 [GeForce GTX 1080] (rev a1)
03:00.1 Audio device: NVIDIA Corporation GP104 High Definition Audio Controller (rev a1)
```

The second card (slotted at 03:00.x) needs to be bound to `vfio-pci`.

For this you can run `moth bind 0000:03:00.0 vfio-pci` (don't forget to prepend the slot with `0000:`), this will create a udev rule which will invoke `moth bind-to 0000:03:00.0 vfio-pci` when this card is added to the system at that slot.

when `moth bind-to 0000:03:00.0 vfio-pci` is called, it will first try to load the kernel module for that given device. then if needed will unbind the PCI device from the kernel module, and bind it to `vfio-pci`.

It loads the default kernel module first because when it tries to add the card to `vfio-pci` it will immeditially bind to other cards that are not claimed yet, which is not desired.
