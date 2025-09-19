## Access Tokens
When a user authenticates to a system, their password is checked and if verified, an access token is assigned. 
Every time a user interacts with a process, a copy of this token will be presented to determinate their privilege level.

## Enumerating Network Services
```
netstat -ano
```
- Look for active network connections listening on loopback addresses 172.0.0.1 and ::1 that are not listening on the IP address or broadcast (0.0.0.0, ::/0)

**Get Name of Process
```
tasklist /fi "PID eq <PID>"
```

**Examples
- Splunk Universal Forwarder used to run as SYSTEM$ and had no authentication
- Erlang Port (25672) weak cookie or cookie placed in accessible configuration file. solarWinds is an Erlang application.

## Named Pipes
Pipes are essentially files stored in memory that get cleared out after being read.
Cobalt Strike workflow:
1. Beacon starts a named pipe of \.\pipe\msagent_12
2. Beacon starts a new process and injects command into that process directing output to \.\pipe\msagent_12
3. Server displays what was written into \.\pipe\msagent_12

In this way, if the command being ran got flagged by antivirus or crashed, it would not affect the beacon (process running the command). To masquerade, rename pipe.

Example of a named pipe: \\.\\PipeName\ExampleNamedPipeServer.
Windows uses a client-server implementation. Half-duplex is a one-way channel from client to server. Duplex is both way communication.

**Listing Named Pipes with Pipelist: https://docs.microsoft.com/en-us/sysinternals/downloads/pipelist
```
pipelist.exe /accepteula
```

**Listing Named Pipes with PowerShell
```
gci \\.\pipe\
```

**Reviewing LSASS Named Pipe Permissions: https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk
```
accesschk.exe /accepteula \\.\Pipe\lsass -v
```

**Checking WindscribeService Named Pipe Permissions
```
accesschk.exe -accepteula -w \pipe\WindscribeService -v
```
- FILE_ALL_ACCESS for Everyone!
