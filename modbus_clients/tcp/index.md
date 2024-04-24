# SHM Modbus - Modbus TCP Client

The ```modbus-tcp-client-shm``` is a simple command line based Modbus TCP client for POSIX compatible operating systems that stores the contents of its registers in shared memory.

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
modbus-tcp-client-shm [OPTION...]
```

Network options:

- ```-i, --host```: Specifies the host to listen for incoming connections. If not specified, all connections are accepted.
- ```-p, --service```: Specifies the service or port to listen for incoming connections. If not specified, the default Modbus port (502) is used.
- ```-c, --connections```: Specifies the number of allowed simultaneous Modbus Server connections. If not specified, only one Modbus Server can connect to the client.
- ```-r, --reconnect```: If set, the client does not terminate if no more Modbus Servers are connected.
- ```-t, --tcp-timeout```: Specifies the TCP timeout in seconds. Set to 0 to use system defaults. If no timeout is specified, the TCP timeout is set to five seconds.

> **Note**:  
The Modbus default port can not be used as unprivileged user.
See [Use privileged ports](#Use-privileged-ports) for details.

Shared memory options:

- ```-n, --name-prefix```: Specifies the shared memory name prefix. If not specified, the default prefix ```modbus_``` is used.
- ```-s, --separate```: Uses a separate shared memory for requests with the specified client id. 
Multiple client ids can be specified by separating them with ','.
Use ```--separate-all``` to generate separate shared memories for all possible client ids.
- ```--separate-all```: Generates separate shared memories for all possible client ids (creates 1024 shared memory files).
- ```--semaphore```: Protects the shared memory with a named semaphore against simultaneous access.
- ```-b, --permissions```: Specifies the permission bits applied when creating a shared memory. If not specified, the permission bits 0640 are applied (user can read and write, group can read).

> **Note**:  
```--separate``` and especially ```--separate-all``` creates a lot of files.
This might exceed the maximum user file limit.
See [common problems](../../common_problems.md).

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

#### Example 1

```bash
modbus-tcp-client-shm
```

Start client and listen to all incoming connections on the Modbus standard port (502).

#### Example 2

```bash
modbus-tcp-client-shm -p 10000 -r
```

Start client and listen to all incoming connections on port 10000.
The client does not terminate if the Server disconnects.

#### Example 3

```bash
modbus-tcp-client-shm -p 10000 -i 127.0.0.1
```

Start client and listen to incoming connections via ip 127.0.0.1 on port 10000.

### Use privileged ports

Ports below 1024 cannot be used by standard users.
Therefore, the default Modbus port (502) cannot be used without further action.

Here are two ways to use the port anyway:

#### iptables (recommended)

An entry can be added to the iptables that forwards the packets on the actual port to a higher port.

The following example redirects all tcp packets on port 502 to port 5020:
```
iptables -A PREROUTING -t nat -p tcp --dport 502 -j REDIRECT --to-port 5020
```

#### setcap

The command ```setcap``` can be used to allow the application to access privileged ports.
However, this option grants the application significantly more rights than it actually needs and should therefore be avoided.

This option cannot be used with flatpaks.

```bash
setcap 'cap_net_bind_service=+ep' </path/to/binary>
```

## Install

### Using the Arch User Repository (recommended for Arch based Linux distributions)

The application is available as [modbus-tcp-client-shm](https://aur.archlinux.org/packages/modbus-tcp-client-shm) in the [Arch User Repository](https://aur.archlinux.org/).
See the [Arch Wiki](https://wiki.archlinux.org/title/Arch_User_Repository) for information about how to install AUR packages.

### Using the Modbus Collection Flatpak Package: SHM Modbus

[SHM Modbus](https://nikolask-source.github.io/SHM_Modbus/) is a collection of multiple tools to simulate a Modbus client.
It is available via Flathub as [network.koesling.shm-modbus](https://flathub.org/apps/network.koesling.shm-modbus).

```modbus-tcp-client-shm``` is invoked by executing the following command:
```
flatpak run network.koesling.shm-modbus modbus-tcp-client-shm
```
