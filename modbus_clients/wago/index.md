# SHM Modbus - Modbus Server for WAGO Modbus Coupler

This application is included in the [SHM Modbus](../../index.md) collection.

```wago-modbus-coupler-shm``` is a Modbus server application designed to connect to a WAGO Modbus TCP coupler.
It retrieves values from the Modbus coupler and stores them in shared memory for easy access by other applications.

The Server creates four shared memories. 
One for each register type:
- Coils (DO)
- Discrete Inputs (DI)
- Holding Registers (AO)
- Input Registers (AI)

## Usage

```text
wago-modbus-coupler-shm [OPTION...] host [service]
```

- ```-q, --quiet```: Disable output.
- ```-d, --debug```: Enable Modbus debug output.
- ```-c, --cycle```: Set cycle time in milliseconds (default: 0; as fast as possible).
- ```--no-cycle-time-fail```: Do not fail if the cycle time is repeatedly exceeded.
- ```--no-cycle-time-warn```: Do not print a warning if the cycle time is exceeded.
- ```--read-start-image```: Do not initialize output registers with zero, but read values from the Modbus coupler.
- ```-p, --prefix```: Name prefix for the shared memories (default: wago_).

Options:

Positional arguments:

- ```host```: Host or address of the WAGO Modbus TCP Coupler.
- ```service```: Service or port of the WAGO Modbus TCP Coupler (default: 502).

### Example

#### Example 1

```bash
wago-modbus-coupler-shm 192.168.1.100
```

Connects to the WAGO Modbus TCP Coupler at IP address ```192.168.1.100```.
All outputs of the Modbus coupler are set to ```0```.

#### Example 2

```bash
wago-modbus-coupler-shm --read-start-image 192.168.1.100
```

Connects to the WAGO Modbus TCP Coupler at IP address ```192.168.1.100```.
All outputs of the Modbus coupler are not changed.

## Install

### Using the Arch User Repository (recommended for Arch based Linux distributions)

The application is available as [wago-modbus-coupler-shm](https://aur.archlinux.org/packages/wago-modbus-coupler-shm) in the [Arch User Repository](https://aur.archlinux.org/).
See the [Arch Wiki](https://wiki.archlinux.org/title/Arch_User_Repository) for information about how to install AUR packages.

### Using the Modbus Collection Flatpak Package: SHM Modbus

[SHM Modbus](https://nikolask-source.github.io/SHM_Modbus/) is a collection of multiple tools to simulate a Modbus client.
It is available via Flathub as [network.koesling.shm-modbus](https://flathub.org/apps/network.koesling.shm-modbus).

```wago-modbus-coupler-shm``` is invoked by executing the following command:
```
flatpak run network.koesling.shm-modbus wago-modbus-coupler-shm
```
