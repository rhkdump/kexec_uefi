# Goal of the project
As UEFI PE format kernel image is more popular, it urges a bailout to kexec reboot those kinds of images.

That method should support various PE format, an ideal way is to obey UEFI specification. As a result efi-stub and the method communicate through UEFI specification, which is more reliable.

When archieving the goal, it should keep the following in mind:

## limited ambitions
The kexec uefi boot loader is not an alternative to uefi boot loader such as edk2, so we need to define a boundary for its functionality, which help us to keep code simple.

A dedicated document 'uefi_subset.md' records the minimal requirement of uefi

## flexible
The boot loader should support various kernel format, at present, aarch64-PE, aarch64-zboot and UKI

## extendable
The boot loader can support some hardwares, but this requirement sort of conflict to the "limited ambitions"

# The competitive solutions:

## 1. UKI format parser in linux kernel
pros and cons:
- Straight forward and easy to implement.
- resident in kernel, free of security risk.
- For aarch64, the zboot format parser only resides in the user space. So if a zboot kernel in UKI .linux section, it has trouble to continue the second stage parsing.
[refer to kexec-uki][1]

## 2. the emulator in user space 
pros and cons:
- Easy to debug and sort of extendable because at this stage, the hardware's states are owned by linux kernel, which should not be changed unexpectedly.
- Risk of security.
[refer to efilite][2]

## 3. the emulator of purgatory-like style
pros and cons:
- Resident in kernel, free of the secuirty risk.
- Owning the hardware exclusively at this stage, it can change hardware's states at its will. But it is not wise to re-write more device drivers
- Hard to debug at present, but can be resolved by saving the context.
[refer to branch kexec_uefi_emulator_RFCv2][3]

# Reference
Information extracted from the email thread [[RFCv2 0/9] UEFI emulator for kexec][4]

[1]: https://github.com/Cydox/linux/blob/2908db6d8556fa617298cfb713355edaa9e4b095/arch/x86/kernel/kexec-uki.c
[2]: https://github.com/ardbiesheuvel/efilite.git
[3]: https://github.com/pfliu/linux/tree/kexec_uefi_emulator_RFCv2
[4]: https://lore.kernel.org/lkml/20240819145417.23367-1-piliu@redhat.com/T/
