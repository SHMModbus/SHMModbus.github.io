# Shared Memory Modbus Client Simulator - Common Problems

Go back to the [SHM Modbus](index.md) main page.

## Failed to create Shared Memory or Semaphore (Modbus Client: TCP and RTU)

It can happen that the client reports the following error on startup:
```
Failed to create shared memory ...: File exists
```
This can be caused by:
 - Another Modbus client is running that uses the shared memory with the given name.  
   If you want to run multiple instances simultaneously use the option ```--name-prefix``` to change the name of the shared memory.
 - Any other application uses a shared memory with the given name (unlikely but possible)  
   Close the Application that created the shared memory or use the option ```--name-prefix``` to change the name of the Modbus shared memory.
 - A previous instance of a Modbus client crashed or was forcefully terminated (e.g. by SIGKILL) and was not able to unlink the shared memory.  
   In this case, the option ```--force``` can be used to force the use of shared memory.
   In the other cases this option should not be used and **should never be used as a "default option"**.

A similar error can occur when creating the semaphore.
In this case, change the semaphore name or if necessary use the option ```--semaphore-force```.

## Connection frequently times out (Modbus Client: TCP)

If the connection frequently times out, it may be reasonable to increase the tcp timeout with the option ```--tcp-timeout```.
Its default value is 5 seconds.

The two options ```--byte-timeout``` and ```--response-timeout``` change the timeout behavior of the Modbus connection. 
These should only be changed by experienced users.
See the [libmodbus documentation](https://libmodbus.org/reference/) ([byte timeout](https://libmodbus.org/reference/modbus_set_byte_timeout/) and [response timeout](https://libmodbus.org/reference/modbus_set_response_timeout/)) for more details.

## No Permission to create a TCP Socket (Modbus Client: TCP)

Ports below 1024 are privileged ports and therefore usually can not be used by standard users.

An entry can be added to the iptables that forwards the packets on the actual port to a higher port.
The following example redirects all tcp packets on port 502 to port 5020:
```
iptables -A PREROUTING -t nat -p tcp --dport 502 -j REDIRECT --to-port 5020
```
The Modbus client must than be started with the redirection target port.
```
... -p 5020
```

Alternatively you can disable the privileged ports by executing the following command as root:
```
sysctl net.ipv4.ip_unprivileged_port_start=0
```

## Too many open files (Modbus Client: TCP *and RTU*)

It may happen that the start of the Modbus client fails with the error message ```Failed to open shared memory ...: Too many open files```.
This is very unlikely in general use and usually only occurs if the TCP client is started with the option ```--separate``` or ```--separate-all```.

To resolve this issue, the user's open file limit can be adjusted.
Use 
```
ulimit -n
```
to show the current file limit and
```
ulimit -n <new_limit>
```
to set a new open file limit.
