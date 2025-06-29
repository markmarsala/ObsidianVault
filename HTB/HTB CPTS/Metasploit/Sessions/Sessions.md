Start session in background:
[CTRL] + [Z]
OR
background (in Meterpreter)

```shell-session
sessions
```

Interact with session:
```shell-session
sessions -i 1
```

Now, you can run post-exploitation modules against a session using 'show options'


#### Jobs

```shell-session
jobs -h
```

Run an exploit as a job:
exploit -j


