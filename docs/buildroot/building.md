---
tags:
  - SATOS
  - Basics
---
# Building
This document will cover how to do more advanced build tasks outside of the SATOS makefile environment.

## Buildroot make environment
The original make environment of Buildroot is accessible by entering the `output` directory.

??? Info "Enter the environment"
      
      ```bash
      cd output
      # run your make commands
      ```

## Rebuilding single packages
This is required in the event of extensive package changes and comes in handy during development.

If you want to rebuild linux, the following commands can be executed inside the BR make environment.
```bash
# Clean linux directories
make linux-dirclean
# Rebuild the linux kernel
make linux-rebuild
```

## Adjusting configurations
Certain packages have their own configuration frontends / guis.

### Buildroot
The main configuration framework from within Buildroot can be opened using
```bash
make menuconfig
```

### Linux Kernel
Linux configuration can be adjusted by running.
```bash
make linux-menuconfig
```

### U-Boot
U-Boot configuration can be adjusted with
```bash
make uboot-menuconfig
```

After adjusting the above configurations, they become temporary for the current build.

### Make changes permanent
Making changes permanent is done by executing the `savedefconfig` fleet of commands.
For Buildroot this is accomplished by running 
```bash
make savedefconfig
```

??? Danger "Todo: Fragment configs"

    Some packages use fragment configurations, only adding a minimal set of configuration options which helps with maintainability.
    It is currently unclear if the below commands for single packages correctly modify these.

For the other packages that support saving their configuration e.g Linux and U-Boot, this is done by 
running the `savedefconfig` command with the package prefix.

Examples:
```bash
# Save the UBoot defconfig
make uboot-savedefconfig

# Save the Linux defconfig
make linux-savedefconfig
```