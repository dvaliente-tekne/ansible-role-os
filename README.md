# OS Role

Configures Arch Linux system settings including locale, timezone, pacman mirrors, and EFI boot entries.

## Requirements

- Arch Linux system
- EFI boot (for boot entry management)
- `reflector` package installed

## Role Variables

### Host Configuration

Host-specific settings are defined in `host_configs` dictionary:

| Host | Kernel | Drive | Special Params |
|------|--------|-------|----------------|
| THEMIS | linux | nvme0n1 | Standard |
| ASTER | linux-tkg-alk | nvme0n1 | enable_guc=3 |
| HEPHAESTUS | linux-tkg-ntl | sda | Standard |
| default | linux-tkg-ntl | nvme0n1 | Standard |

### Locale & Timezone

| Variable | Default | Description |
|----------|---------|-------------|
| `os_locale` | `en_US.UTF-8` | System locale |
| `os_timezone` | `America/El_Salvador` | Timezone |

### Mirror Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `os_mirror_country` | `United States` | Country for mirrors |
| `os_mirror_count` | `100` | Number of mirrors |
| `os_mirror_protocols` | `https,ftp` | Allowed protocols |
| `os_mirror_age` | `24` | Max mirror age in hours |

### EFI Boot Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `os_manage_efi` | `true` | Manage EFI boot entries |
| `os_delete_existing_boot_entry` | `true` | Delete entry before creating |
| `os_boot_entry_to_delete` | `0` | Boot entry number to delete |
| `os_efi_label` | `BOOT` | Label for boot entry |
| `os_efi_partition` | `1` | EFI partition number |
| `os_microcode` | `intel-ucode` | CPU microcode package |

## Tags

| Tag | Description |
|-----|-------------|
| `os` | All OS tasks |
| `config` | Host configuration resolution |
| `locale` | Locale settings |
| `timezone` | Timezone and NTP |
| `mirrors` | Pacman mirrorlist |
| `efi` | EFI boot entry management |

## Example Playbook

```yaml
- hosts: localhost
  roles:
    - role: os
      vars:
        os_locale: de_DE.UTF-8
        os_timezone: Europe/Berlin
        os_manage_efi: false
```

## Adding a New Host

Add to `host_configs` in defaults:

```yaml
host_configs:
  NEWHOSTNAME:
    kernel: linux-zen
    drive: nvme1n1
    params: "your kernel params"
```

## Safety Notes

- EFI operations require root privileges
- `os_manage_efi: false` disables all boot entry changes
- Test with `--check` before running on production systems

## License

MIT

## Author

dvaliente
