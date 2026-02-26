# AreaDetector Modules Configuration for EPICS-env

This repository configures and builds AreaDetector modules.
It is designed to work seamlessly with [EPICS-env](https://github.com/jeonghanlee/EPICS-env).

## Modules

The following modules will be installed:

* ADCore
* ADSimDetector
* ADGenICam
* ADVimba

## System Requirements

This configuration manages dependencies via system packages and does not use `ADSupport`.

**Required Linux Packages:**

```bash
# Install dependencies
sudo apt install libxml2-dev libhdf5-dev libjpeg-dev libtiff-dev libz-dev libusb-1.0-0-dev
```

## Installation

### Option 1: Using EPICS-env (Recommended)

If `EPICS-env` is set up, `EPICS_BASE` is used to configure `CONFIG_SITE.local`.
**Note:** `make conf.base` must be executed first to generate the configuration file before initialization.

```bash
# 1. Source the EPICS environment
source <path_to_epics_version>/setEpicsEnv.bash

# 2. Configure and Build
make conf.base
make init
make conf
make build
make symlinks
```

### Option 2: Manual Configuration

If not using `EPICS-env`, `INSTALL_LOCATION` must be defined manually.

```bash
# 1. Configure the installation location
# Replace <absolute_path_to_epics_base> with the actual path.
echo "INSTALL_LOCATION=<absolute_path_to_epics_base>" > configure/CONFIG_SITE.local

# 2. Build
make init
make conf
make build
make symlinks
```
