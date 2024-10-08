# subset of uefi needed
Since the goal is to support the kexec-reboot, implementing UEFI entirely is intractable, and unnecessary.
Implementing the subset of UEFI that is actually needed to boot Linux *is* tractable.
  - the EFI stub needs the boot services for the EFI memory map and the allocation routines
  - GRUB needs block I/O
  - systemd-stub/UKI needs file I/O to look for sidecars
  - systemd-stub needs to handle TPM carefully
