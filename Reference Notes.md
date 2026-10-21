# Platform Trust

| Check | Source |
| ---- | ---- |
| UEFI | Directory `/sys/firmware/efi/` exists |
| Secure Boot	| `mokutil --sb-state`, SecureBoot EFI variable in `/sys/firmware/efi/efivars/` |
| Signing Keys | `mokutil --db` / `--list-enrolled` / `--list-new`, `efi-readvar`, `/proc/keys/` |
| Module Signing | Get Kernel's Observed Security State, See table for module auditing |
| TPM 2.0 | `/sys/class/tpm/tpm0/tpm_version_major`, `/sys/class/tpm/`, `/sys/class/tpm/tpm0/`, `/dev/tpm0`, `/dev/tpmrm0`,  `systemd-analyze has-tpm2`|
| Measured boot | `/sys/kernel/security/tpm0/binary_bios_measurements` and PCR values being non-default |

# Runtime Security

| Check | Source |
| ---- | ---- |
| IOMMU	| `journalctl -k -b`, `/sys/kernel/iommu_groups/`, `/proc/cmdline`, `/sys/class/iommu/`, `/sys/kernel/iommu_groups/*/devices/` |
| Lockdown | `/sys/kernel/security/lockdown`, `journalctl -k -b --no-pager | grep -i lockdown` |
| Kexec	| `/proc/sys/kernel/kexec_load_disabled` |
| LSM BPF	| `/sys/kernel/security/lsm` contains bpf | 

# Additional Indicators

| Check | Source |
| ---- | ---- |
| MBR vs. GPT | `lsblk -o NAME,TYPE,PTTYPE,FSTYPE,MOUNTPOINTS` |
| Kernel version and feature availability | `uname -r`, `uname -m`, `/proc/cmdline` |
| Systemd and EAC service readiness | `ps -p 1 -o comm=`, `systemctl list-unit-files --type=service` |

# Kernel Module Auditing

| Check | Source |
| ---- | ---- |
| Kernel version | `uname -r` |
| Kernel build configuration | `/proc/config.gz`, `/boot/config-$(uname -r)`, `/lib/modules/$(uname -r)/build/.config` |
| Module signature enforcement | `/proc/sys/kernel/module_sig_enforce`, inspect boot logs and kernel configuration as additional evidence | 
| Loaded module inventory	| `/proc/modules`, `/sys/module/`
| Module signature metadata |	`modinfo -F signer MODULE`, `modinfo -F sig_id MODULE` |
| Kernel boot trust | Kernel messages in 1journalctl -k -b`, `dmesg`
