# SHM Modbus - Modbus RTU Client

[SHM Modbus](../../index.md) > [Modbus Clients](../index.md) > RTU
---

The ```modbus-rtu-client-shm``` is a simple command line based Modbus RTU client for POSIX compatible operating systems that stores the contents of its registers in shared memory.

The client creates four shared memories. 
One for each register type:
- Coils (DO)
- Discrete Inputs (DI)
- Holding Registers (AO)
- Input Registers (AI)

All registers are initialized with 0 at the beginning.
The values from these registers (shared memory) are directly accessed by the Modbus server.
The actual functionality of the client is realized by applications that read data from or write data to the shared memory.

## Usage

```text
modbus-rtu-client-shm [OPTION...]
```

Serial device options:

- ```-d, --device```: Specifies the serial device to use. (mandatory)
- ```-i, --id```: Sets the Modbus RTU client id. (mandatory)
- ```-p, --parity```: Specifies the serial parity bit. Options are N (None), E (Even), O (Odd). Default is N.
- ```--data-bits```: Sets the number of serial data bits (5-8). Default is 8.
- ```--stop-bits```: Specifies the number of serial stop bits (1-2). Default is 1.
- ```-b, --baud```: Sets the serial baud rate. Default is 9600.
- ```--rs485```: Forces the use of RS485 mode.
- ```--rs232```: Forces the use of RS232 mode.

Shared memory options:

- ```-n, --name-prefix```: Specifies the shared memory name prefix. If not specified, the default prefix ```modbus_``` is used.
- ```--semaphore```: Protects the shared memory with a named semaphore against simultaneous access.
- ```-b, --permissions```: Specifies the permission bits applied when creating a shared memory. If not specified, the permission bits 0640 are applied (user can read and write, group can read).

Modbus options:

- ```--{do,di,ao,ai}-registers```: Specifies the number of registers. If not specified, the client starts with the maximum number of Modbus registers (65536).
- ```-m, --monitor```: Outputs all incoming and outgoing packets to stdout.
- ```--byte-timeout```: Specifies the timeout interval in seconds between two consecutive bytes of the same message.
- ```--response-timeout```: Specifies the timeout interval in seconds used to wait for a response.

> **None**:  
```--monitor``` can have a strong performance impact.

> **Note**:  
Usually there is no need to specify ```--byte-timeout``` and ```--response-timeout```

### Examples

```bash
modbus-rtu-client-shm -d /dev/ttyUSB0 -i 1 --baud 9600
```

This command initializes the Modbus RTU client with Modbus client id ```1```, using serial communication via the device ```/dev/ttyUSB0``` with a baud rate of ```9600```.


## Install

### Using the Arch User Repository (recommended for Arch based Linux distributions)

The application is available as [modbus-rtu-client-shm](https://aur.archlinux.org/packages/modbus-rtu-client-shm) in the [Arch User Repository](https://aur.archlinux.org/).
See the [Arch Wiki](https://wiki.archlinux.org/title/Arch_User_Repository) for information about how to install AUR packages.

### Using the Modbus Collection Package: SHM Modbus

[SHM Modbus](https://nikolask-source.github.io/SHM_Modbus/) is a collection of multiple tools to simulate a Modbus client.

#### Flatpak

The Flatpak package is available via Flathub as [network.koesling.shm-modbus](https://flathub.org/apps/network.koesling.shm-modbus).

```modbus-rtu-client-shm``` is invoked by executing the following command:

```
flatpak run network.koesling.shm-modbus modbus-rtu-client-shm
```

#### Snap:

The snap package can be downloaded via the [github release page](https://github.com/SHMModbus/SHM_Modbus/releases).

```modbus-rtu-client-shm``` is invoked by executing the following command:

```
shm-modbus.modbus-rtu-client-shm
```

