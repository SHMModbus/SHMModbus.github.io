[SHM Modbus](index.md) > Getting Started

# SHM Modbus - Getting Started

"This getting started section demonstrates setting up a Shared Memory Modbus client and simulating register data."

## 1. Start Modbus client

```bash
modbus-tcp-client-shm --semaphore modbus -p 5020
```

This command starts a shared memory Modbus TCP client listening on port ```5020```.
It creates the shared memories ```modbus_DO```, ```modbus_DI```, ```modbus_AO``` and ```modbus_AI``,
and creates the semaphore ```modbus``` to protect the shared memory from simultaneous access.

For more details about using the Modbus clients see [modbus-tcp-client-shm](modbus_clients/tcp/index.md) and [modbus-rtu-client-shm](modbus_clients/rtu/index.md).

## 2. Interact with the shared memory

SHM Modbus provides tools to inspect and manipulate the Modbus client data.

### Write data

- [write-shm](shm_tools/write_shm/index.md): write any data to a shared memory. (available via the GUI)
- [shared-mem-random](shm_tools/shared_mem_random/index.md): write random data to a shared memory (available via the GUI)
- [stdin-to-modbus-shm](shm_modbus/stdin_to_modbus_shm/index.md): write values (with specified data type) to registers.
- [shm-modbus-signal-gen](shm_modbus/signal_gen/index.md): signal generator that generates input for [stdin-to-modbus-shm](shm_modbus/stdin_to_modbus_shm/index.md)

### Inspect data

- [dump-shm](shm_tools/dump_shm/index.md): outputs shared memory data to stdout. Can be used in combination with the ```hexdump``` command to create a hexdump of a shared memory. (The hexdump functionality is available via the GUI.)
- [shm-format](shm_tools/shm_format/index.md): reads data with a specified data type from shared memory. 
