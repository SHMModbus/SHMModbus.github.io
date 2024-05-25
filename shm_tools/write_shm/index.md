# SHM Modbus - Shared Memory Writer

The ```write-shm``` writes the content from its standard input to a named shared memory.
It provides options to specify the shared memory name and optionally protect it with an existing named semaphore against simultaneous access.
Additionally, options are available to invert input bits, repeat input if the input size is smaller than the shared memory, and pass through all data written to the shared memory to the standard output.

## Usage

```text
write-shm [OPTION...]
```

Options:

- ```-n, --name```: Specifies the name of the shared memory (mandatory)
- ```-s, --semaphore```: Enables protection of the shared memory with an existing named semaphore to prevent simultaneous access.
- ```-i, --invert```: Inverts all input bits
- ```-r, --repeat```: Repeats input until the shared memory is completely filled.
    This option has no effect if the input size is greater or equal the size of the shared memory. 
- ```-p, --passthrough```: Outputs everything that is written to the shared memory to standard output.


### Examples

#### Example 1
```bash
echo "Hello, World!" | write-shm -n my_shared_memory
```

Writes ```"Hello, World!"``` to a shared memory named ```my_shared_memory```.

#### Example 2
```bash
write-shm -n my_shm -s my_sem < data.bin
```

Write the binary file ```data.bin``` to a shared memory segment named ```my_shm``` and protect it with the semaphore ```my_sem```.

#### Example 3
```bash
write-shm -n my_shm < /dev/zero
```

This command writes an infinite stream of null bytes from /dev/zero to the shared memory segment named ```my_shm```.
Therefore, every bit of ```my_shm``` is set to ```0```.

#### Example 4
```bash
write-shm -n my_shm -i < /dev/zero
```

This command writes an infinite stream of inverted null bytes from /dev/zero to the shared memory segment named ```my_shm```.
Therefore, every bit of ```my_shm``` is set to ```1```.


#### Example 5
```bash
write-shm -n my_shm -s my_sem < /dev/urandom > data.bin
```

This command writes a stream of pseudorandom bytes from ```/dev/urandom``` to the shared memory segment named ```my_shm``` while protecting it with the semaphore ```my_sem```.
Additionally, it saves the same data to a file named ```data.bin```.

#### Example 6
```bash
write-shm -n my_shm -s my_sem -r < small_data.bin
```

This command writes the content of the file ```small_data.bin``` to the shared memory segment named ```my_shm```.
If the size of ```small_data.bin``` is smaller than the shared memory size, the input will be repeated to fill the shared memory segment.
Additionally, it will be protected by the semaphore ```my_sem``` to prevent simultaneous access.

## Install

### Using the Arch User Repository (recommended for Arch based Linux distributions)

The application is available as [write-shm](https://aur.archlinux.org/packages/write-shm) in the [Arch User Repository](https://aur.archlinux.org/).
See the [Arch Wiki](https://wiki.archlinux.org/title/Arch_User_Repository) for information about how to install AUR packages.

### Using the Modbus Collection Flatpak Package: SHM Modbus

[SHM Modbus](https://nikolask-source.github.io/SHM_Modbus/) is a collection of multiple tools to simulate a Modbus client.
It is available via Flathub as [io.github.shmmodbus.shm-modbus](https://flathub.org/apps/io.github.shmmodbus.shm-modbus).

```write-shm``` is invoked by executing the following command:
```
flatpak run io.github.shmmodbus.shm-modbus write-shm
```
