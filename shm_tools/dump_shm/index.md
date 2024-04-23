# Shared Memory Dump

The ```dump-shm``` application is designed to output the content of a shared memory to its standard output. 
It provides options to specify the shared memory name, limit the number of bytes to output and adjust the offset.
In addition the shared memory access can be protected against simultaneous access with an existing named semaphore.


## Usage

```text
dump-shm [OPTION...] SHM_NAME
```

Options:

- ```-b, --bytes```: Limits the number of bytes to output.
- ```-o, --offset```: Specifies the number of bytes to skip.
- ```-s, --semaphore```: Enables protection of the shared memory with an existing named semaphore to prevent simultaneous access.

### Example
```bash
dump-shm -b 64 -o 32 -s my_sem my_shm
```

This example will dump the content of the shared memory named "my_shm" to the standard output, starting from the 33rd byte (skipping the first 32 bytes), and outputting a maximum of 64 bytes while ensuring exclusive access to the shared memory using the semaphore "my_sem".

## Install

### Using the Arch User Repository (recommended for Arch based Linux distributions)

The application is available as [dump-shm](https://aur.archlinux.org/packages/dump-shm) in the [Arch User Repository](https://aur.archlinux.org/).
See the [Arch Wiki](https://wiki.archlinux.org/title/Arch_User_Repository) for information about how to install AUR packages.

### Using the Modbus Collection Flatpak Package: SHM Modbus

[SHM Modbus](https://nikolask-source.github.io/SHM_Modbus/) is a collection of multiple tools to simulate a Modbus client.
It is available via Flathub as [network.koesling.shm-modbus](https://flathub.org/apps/network.koesling.shm-modbus).

```dump-shm``` is invoked by executing the following command:
```
flatpak run network.koesling.shm-modbus dump-shm
```
