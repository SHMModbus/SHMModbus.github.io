# SHM Modbus

This project is a a collection of applications to simulate a Modbus TCP/RTU client. 

It contains the following tools:

- General Shared Memory Tools:
  - [Shared Memory Dump](shm_tools/dump_shm/)
  - [Shared Memory Write](shm_tools/write_shm/)
  - [Shared Memory Randomize](shm_tools/shared_mem_random/)
  - [Shared Memory Formatter](shm_tools/shm_format/)
- Modbus Clients:
  - [RTU](modbus_clients/rtu/)
  - [TCP](modbus_clients/tcp/)
- Modbus Shared Memory Tools:
  - [STDIN to Modbus SHM](shm_modbus/stdin_to_modbus_shm/)
  - [SHM Modbus Signal Generator](shm_modbus/signal_gen/)

In addition, a [QT6 GUI](shm_modbus/gui/) is included.
It provides a subset of all SHM Modbus features.

## Install

### Using the Arch User Repository (recommended for Arch based Linux distributions)

The application is available as [shm-modbus](https://aur.archlinux.org/packages/shm-modbus) in the [Arch User Repository](https://aur.archlinux.org/).
See the [Arch Wiki](https://wiki.archlinux.org/title/Arch_User_Repository) for information about how to install AUR packages.

### Snap

The application is available as snap package.
You can download it via the [github releases page](https://github.com/SHMModbus/SHM_Modbus/releases).

### Flatpak

The application is available as flatpak and published on flathub as ```network.koesling.shm-modbus```.

To install from the command line, use the following command:
 
```bash
flatpak install network.koesling.shm-modbus
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

