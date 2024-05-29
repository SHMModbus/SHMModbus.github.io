# SHM Modbus - Shared Memory Randomize

[SHM Modbus](../../index.md) > [Modbus Tools](../index.md) > random
---

The ```shared-mem-random``` application writes random values to a shared memory.
It can use an existing shared memory or create a new one.

## Usage

### Shared Memory Options

- ```-n, --name```: Mandatory name of the shared memory object.
- ```-c, --create```: Creates a shared memory segment with the given size in bytes.
- ```--force```: Forces the creation of shared memory even if it already exists. This option is only relevant if -c is used.
- ```-p, --permissions```: Specifies the permission bits applied when creating a shared memory segment. The default permission bits are 0660.
- ```--semaphore```: Protects the shared memory with a named semaphore against simultaneous access. If -c is used, the semaphore is created; otherwise, an existing semaphore is required.
- ```--semaphore-force```: Forces the use of the semaphore even if it already exists. This option should only be used if the semaphore of an improperly terminated instance continues to exist as an orphan and is no longer used. This option is only relevant if -c is used.
- ```--pid```: Terminates the application if the application with the given process ID (pid) is terminated.

### Other Options

- ```-a, --alignment```: Specifies the byte alignment to generate random values. Possible values are 1, 2, 4, or 8. The default alignment is 1.
- ```-m, --mask```: Optional bitmask that is applied to the generated random values.
- ```-i, --interval```: Specifies the interval for generating random values in milliseconds. The default interval is 1000 milliseconds.
- ```-l, --limit```: Sets a limit on the random interval. Use 0 for no limit, which means the application will run until it receives a SIGINT or SIGTERM signal. The default limit is 0.
- ```-o, --offset```: Specifies the number of bytes to skip at the beginning of the shared memory. The default offset is 0.
- ```-e, --elements```: Sets the maximum number of elements to work on. The element size depends on the specified alignment.

### Examples

#### Example 1

```bash
shared-mem-random -n my_shm -c 1024 -a 2 -i 500
```

This command creates a shared memory named ```my_shm``` with a size of ```1024``` bytes, generating random values aligned on a 2-byte boundary at intervals of 500 milliseconds.

#### Example 2

```bash
shared-mem-random -n my_shm -s my_sem -l 1
```

This command generates random values and writes them once to a shared memory named ```my_shm```, while using an existing semaphore named ```my_sem``` for protection against simultaneous access.

#### Example 3

```bash
shared-mem-random -n modbus_DO -s modbus -a 1 -b 1 --pid $(pidof shared-mem-random)
```

This command generates random values and writes them to a shared memory named ```modbus_DO```, utilizing a semaphore named ```modbus``` for protection against simultaneous access. 
Random values are generated with a byte alignment of 1 at intervals of 500 milliseconds, and a bit mask of 1 is applied to the generated random values. 
Thus, only the lowest bit of each byte is randomized. 
Additionally, it sets up termination of the shared-mem-random application if the process with the PID of the ```shared-mem-random``` application terminates.

#### Example 4

```bash
shared-mem-random -n my_shm -a 8 -o 64 -e 8 -l 1
```

This command configures the ```shared-mem-random``` application to write random values to a shared memory named ```my_shm```.
The random values are generated with an 8-byte alignment and are written starting from an offset of 64 bytes within the shared memory.
The application processes a maximum of 8 elements and runs for a single iteration, generating random values only once before exiting.

## Install

### Using the Arch User Repository (recommended for Arch based Linux distributions)

The application is available as [shared-mem-random](https://aur.archlinux.org/packages/shared-mem-random) in the [Arch User Repository](https://aur.archlinux.org/).
See the [Arch Wiki](https://wiki.archlinux.org/title/Arch_User_Repository) for information about how to install AUR packages.

### Using the Modbus Collection Package: SHM Modbus

[SHM Modbus](https://nikolask-source.github.io/SHM_Modbus/) is a collection of multiple tools to simulate a Modbus client.

#### Flatpak

The Flatpak package is available via Flathub as [io.github.shmmodbus.shm-modbus](https://flathub.org/apps/io.github.shmmodbus.shm-modbus).

```shared-mem-random``` is invoked by executing the following command:

```
flatpak run io.github.shmmodbus.shm-modbus shared-mem-random
```

#### Snap

The snap package can be downloaded via the [github release page](https://github.com/SHMModbus/SHM_Modbus/releases).

```shared-mem-random``` is invoked by executing the following command:

```
shm-modbus.shared-mem-random
```

