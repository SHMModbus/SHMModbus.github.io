# SHM Modbus

This project is a a collection of applications to simulate a Modbus TCP/RTU client. 

It contains the following tools:

- [General Shared Memory Tools](shm_tools/index.md):
  - [Shared Memory Dump](shm_tools/dump_shm/index.md)
  - [Shared Memory Write](shm_tools/write_shm/index.md)
  - [Shared Memory Randomize](shm_tools/shared_mem_random/index.md)
  - [Shared Memory Formatter](shm_tools/shm_format/index.md)
- [Modbus Clients](modbus_clients/index.md):
  - [RTU](modbus_clients/rtu/index.md)
  - [TCP](modbus_clients/tcp/index.md)
  - [WAGO (Modus Server)](modbus_clients/wago/index.md)
- [Modbus Shared Memory Tools](shm_modbus/index.md):
  - [STDIN to Modbus SHM](shm_modbus/stdin_to_modbus_shm/index.md)
  - [SHM Modbus Signal Generator](shm_modbus/signal_gen/index.md)

In addition, a [QT6 GUI](shm_modbus/gui/index.md) is included.
It provides a subset of all SHM Modbus features.

## Install

### Using the Arch User Repository (recommended for Arch based Linux distributions)

The application is available as [shm-modbus](https://aur.archlinux.org/packages/shm-modbus) in the [Arch User Repository](https://aur.archlinux.org/).
See the [Arch Wiki](https://wiki.archlinux.org/title/Arch_User_Repository) for information about how to install AUR packages.

### Snap

The application is available as snap package.
You can download it via the [github releases page](https://github.com/SHMModbus/SHM_Modbus/releases).

1. download snap file
2. install: ```sudo snap install --dangerous <shm-modbus_....snap>```
3. connect plugs:
    - ```sudo snap connect shm-modbus:shm``` 
    - ```sudo snap connect shm-modbus:serial-port``` 

### Flatpak

The application is available as flatpak and published on flathub as ```io.github.shmmodbus.shm-modbus```.

To install from the command line, use the following command:

```bash
flatpak install io.github.shmmodbus.shm-modbus
```

## Use

A short [Getting Started](getting_started.md) guide can be found [here](getting_started.md).

### Memory layout

Each instance of a Modbus client ([TCP](modbus_clients/tcp/) and [RTU](modbus_clients/rtu/)) creates 4 shared memories.
Two of them store discrete inputs and coils (binary values). 
The other two store the Modbus registers. 
(See the [Modbus specification](https://modbus.org/docs/Modbus_Application_Protocol_V1_1b3.pdf) for details.)

Each binary value (discrete input or coil) is represented by one byte in the shared memory.
As they are representing binary values, every non zero value of the byte is interpreted as 1.

The 16 bit Modbus registers are stored as 16 bit values (16 bit aligned) in the shared memory.

## Common Problems

A list of common problems can be found [here](common_problems.md).

