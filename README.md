# bobs.py

Barely Operational Backup Script (or bobs.py) is a simple backup script that allows you to copy files from multiple directories into a single destination using a single toml configuration file.

## Basic Usage

`python3 bobs.py -s -c /path/to/config.toml -d /path/to/destination`

## Options

#### Config

Single destination directory: `[destination]`
- specify files: `files = ["file1", "file2"]`
- specify depth for files: `depth = 2` (/example/path/**to/file.txt**)

Whole directory: `[dirs]`
- specify directory to copy: `"destination/directory" = "/path/to/source/directory"`

#### Flags
```
-h, --help            show this help message and exit
-c, --config CONFIG   set custom config file path (default: ./config.toml)
-d, --destination DESTINATION
                      set custom destination path (default: ./backup)
-s, --skip-unchanged  skip unchanged files in log
-b, --disable-banner  disable decorative banner
-r, --keep-redundant  do not remove redundant files
```

## !important!

Files not found in config.toml (including files in directories set to be copied) get deleted by default. This can be turned off using the `-r` flag to keep redundant files.

## Example usage

#### config.toml
```toml
[config]
depth = 2
files = [
	"/home/user/.config/hypr/hyprland.conf",
	"/home/user/.config/kitty/kitty.conf",
]

[nixos]
depth = 1
files = [
	"/etc/nixos/configuration.nix",
	"/etc/nixos/packages.nix",
]

[dirs]
"scripts" = "/home/user/scripts"
"scripts/configs" = "/home/user/scripts/configs"

```

#### command
`python3 bobs.py -sb -c /home/user/config.toml -d /home/user/destination`
```bash
Config: /home/user/config.toml
Destination: /home/user/destination
[+] /home/user/.config/hypr/hyprland.conf ->  config/hypr/hyprland.conf
[+] /home/user/.config/kitty/kitty.conf ->  config/kitty/kitty.conf
[+] /etc/nixos/configuration.nix ->  nixos/configuration.nix
[+] /etc/nixos/packages.nix ->  nixos/packages.nix
[+] /home/user/scripts/mullvad_toggle.sh ->  scripts/mullvad_toggle.sh
[+] /home/user/scripts/screenshot.sh ->  scripts/screenshot.sh
[+] /home/user/scripts/configs/slurp.conf ->  scripts/configs/slurp.conf
[+] /home/user/scripts/configs/config.toml ->  scripts/configs/config.toml
Created: 8 | Unchanged: 0 | Changed: 0 | Failed: 0 | Removed: 0 
```

#### tree
```
destination/
├─ config/
│  ├─ hypr/
│  │  ├─ hyprland.conf
│  ├─ kitty/
│  │  ├─ kitty.conf
├─ nixos/
│  ├─ configuration.nix
│  ├─ packages.nix
├─ scripts/
│  ├─ configs/
│  │  ├─ config.toml
│  │  ├─ slurp.conf
│  ├─ mullvad_toggle.sh
│  ├─ screenshot.sh
```

## Final notes

This script is very limited and the current config structure doesn't allow for some really basic stuff. I just wrote it to quickly backup some system files, and I'm not planning on expanding it much further. I might add some features, but it's not a guarantee.