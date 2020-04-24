# Setup remote debugger for java on pure IPv6 Network

## Why is this an issue?
Refer: https://docs.oracle.com/javase/8/docs/technotes/guides/jpda/conninv.html
```
The current implementation on the target VM side only supports IPv4, but this could 
change in a future release so that both IPv4 and IPv6 are supported.
```
### How can we circumvent this limitation
One word "Tunnel". Setup a local tunnel on the server from IPV6 [End point 1] network to IPv4 [End point 2] network. Setup a local tunnel on the machine which is used for debugging from IPv4 [End point 3] network to IPv6 [End point 4] network. Configure tunnels such that "End point 1" and "End point 4" can connect over IPv6 network. 

### Assumption
* You have a remote debug enabled IPv6 server (with IP as [2001:db8:85a3:8d3:1319:8a2e:370:7348]) with debug port as 8787
* You have tinyPortMapper/tinyPortForwarder downloaded from https://github.com/wangyu-/tinyPortMapper/releases

### How to enable debugger for IPv6
#### Prepare server machine.
  This exposes port 18787 externally on IPv6 stack and forward all traffic to port 8787 on IPv4 stack on the same machine.
```
call "tinymapper_windows\for winxp and below\tinymapper.exe" -l [::]:18787 -r 127.0.0.1:8787 -t -u
```
#### Prepare machine with source code machine
   This exposes port 8787 externally on IPv4 stack and forward all traffic to port 18787 on IPv6 stack on server machine.
```
call "tinymapper_windows\for winxp and below\tinymapper.exe" -l 0.0.0.0:8787 -r [2001:db8:85a3:8d3:1319:8a2e:370:7348]:18787 -t -u
```
#### Connect debugger on the machine with source code machine
Connect java debugger on socket 127.0.0.1:8787

