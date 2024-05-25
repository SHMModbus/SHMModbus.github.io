# SHM Modbus - STDIN to Modbus Shared Memory

This application reads commands from stdin and writes data to the shared memory created by one of the shared memory Modbus clients ([TCP](../../modbus_clients/tcp/index.md)/[RTU](../../modbus_clients/rtu/index.md)).


## Use the Application

The application can be started, after starting one of the Modbus clients with standard shared memory prefix, without any further arguments.

If the shared memory name prefix has been changed, it can be adjusted via the argument ```----name-prefix```.

After its startup, the application reads commands from stdin. 
Provide one command per line.

The application terminates as soon as no more input data can be read.

### Input format

The commands for the application have the following format:
```
register_type:register_address:value[:data_type]
```

```register_type``` specifies into which register the value should be written.
The following values are possible (case is ignored):
- do
- di
- ao
- ai

```register_address``` specifies the address of the register

```value``` specifies the value that is written.  
The representation depends on the type of modbus register and data type.
For hex and octal numbers the same notation as in C/C++ is used.
Some string constants are available.

```data_type``` optionally specifies a data type.  
If no data type is specified, one register is written in host byte order.  
The following data types are possible:
- Float:
  - 32 Bit:
    - f32_abcd, f32_big, **f32b**  
      32-Bit floating point, big endian
    - f32_dcba, f32_little, **f32l**  
      32-Bit floating point, little endian
    - f32_cdab, f32_big_rev, **f32br**  
      32-Bit floating point, big endian, reversed register order
    - f32_badc, f32_little_rev, **f32lr**  
      32-Bit floating point, little endian, reversed register order
  - 64 Bit:
    - f64_abcdefgh, f64_big, **f64b**  
      64-Bit floating point, big endian
    - f64_ghefcdab, f64_little, **f64l**  
      64-Bit floating point, little endian
    - f64_badcfehg, f64_big_rev, **f64br**  
      64-Bit floating point, big endian, reversed register order
    - f64_hgfedcba, f64_little_rev, **f64lr**  
      64-Bit floating point, little endian, reversed register order
- Integer:
  - 8 Bit:
    - Signed:
      - **i8_lo**  
        8-Bit signed integer written to low byte of register.
        The other byte is set to ```0```
      - **i8_hi**  
        8-Bit signed integer written to high byte of register.
        The other byte is set to ```0```
    - Unsigned:
      - **u8_lo**  
        8-Bit unsigned integer written to low byte of register.
        The other byte is set to ```0```
      - **u8_hi**  
        8-Bit unsigned integer written to high byte of register.
        The other byte is set to ```0```
  - 16 Bit:
    - Signed:
      - i16_ab, i16_big, **i16b**  
        16-Bit signed integer, big endian
      - i16_ba, i16_little, **i16l**  
        16-Bit signed integer, little endian
    - Unsigned:
      - u16_ab, u16_big, **u16b**  
        16-Bit unsigned integer, big endian
      - u16_ba, u16_little, **u16l**  
        16-Bit unsigned integer, little endian
  - 32 Bit:
    - Signed:
      - i32_abcd, i32_big, **i32b**  
        32-Bit signed integer, big endian
      - i32_dcba, i32_little, **i32l**  
        32-Bit signed integer, little endian
      - i32_cdab, i32_big_rev, **i32br**  
        32-Bit signed integer, big endian, reversed register order
      - i32_badc, i32_little_rev, **i32lr**  
        32-Bit signed integer, little endian, reversed register order
    - Unsigned:
      - u32_abcd, u32_big, **u32b**  
        32-Bit unsigned integer, big endian
      - u32_dcba, u32_little, **u32l**  
        32-Bit unsigned integer, little endian
      - u32_cdab, u32_big_rev, **u32br**  
        32-Bit unsigned integer, big endian, reversed register order
      - u32_badc, u32_little_rev, **u32lr**  
        32-Bit unsigned integer, little endian, reversed register order
  - 64 Bit:
    - Signed:
      - i64_abcdefgh, i64_big, **i64b**  
        64-Bit signed integer, big endian
      - i64_hgfedcba, i64_little, **i64l**  
        64-Bit signed integer, little endian
      - i64_ghefcdab, i64_big_rev, **i64br**  
        64-Bit signed integer, big endian, reversed register order
      - i64_badcfehg, i64_little_rev, **i64lr**  
        64-Bit signed integer, little endian, reversed register order
    - Unsigned:
      - u64_abcdefgh, u64_big, **u64b**  
        64-Bit unsigned integer, big endian
      - u64_hgfedcba, u64_little, **u64l**  
        64-Bit unsigned integer, little endian
      - u64_ghefcdab, u64_big_rev, **u64br**  
        64-Bit unsigned integer, big endian, reversed register order
      - u64_badcfehg, u64_little_rev, **u64lr**  
        64-Bit unsigned integer, little endian, reversed register order

> **Note**:  
The endianness refers to the layout of the data in the shared memory and may differ from the Modbus Server's 
definition of the endianness.

> **Note**:  
No data type must be specified for writing the bit registers (DO, DI).

### Command Passthrough

By using the option ```--passthrough```, all valid inputs are written to stdout.
By additionally enabling the option ```--bash```, the output is created as a bash script that reproduces the inputs
(including the timing).

### Protection against simultaneous access

The shared memory Modbus clients can be set up to provide a semaphore as protection mechanism against simultaneous shared memory access. Use the option ```--semaphore``` to use such a semaphore.

> **Note:**  
It is recommended to use the semaphore mechanism as concurrent access to the shared memory may cause data corruption.

### Monitor Modbus Client

The option ```--pid``` is used to specify the process id that ```stdin-to-modbus-shm``` monitors.
If this process terminates, ```stdin-to-modbus-shm``` is terminated.

Usage example:

```bash
stdin-to-modbus-shm --pid $(pidof modbus-tcp-client-shm)
```

> Note: ```stdin-to-modbus-shm``` does not reconnect to a new shared memory if the modbus client is restarted. Thus, using the ```--pid``` option is highly recommended.

## Install

### Using the Arch User Repository (recommended for Arch based Linux distributions)

The application is available as [stdin-to-modbus-shm](https://aur.archlinux.org/packages/stdin-to-modbus-shm) in the [Arch User Repository](https://aur.archlinux.org/).
See the [Arch Wiki](https://wiki.archlinux.org/title/Arch_User_Repository) for information about how to install AUR packages.

### Using the Modbus Collection Flatpak Package: SHM Modbus

[SHM Modbus](https://nikolask-source.github.io/SHM_Modbus/) is a collection of multiple tools to simulate a Modbus client.
It is available via Flathub as [io.github.shmmodbus.shm-modbus](https://flathub.org/apps/io.github.shmmodbus.shm-modbus).

```stdin-to-modbus-shm``` is invoked by executing the following command:
```
flatpak run io.github.shmmodbus.shm-modbus stdin-to-modbus-shm
```
