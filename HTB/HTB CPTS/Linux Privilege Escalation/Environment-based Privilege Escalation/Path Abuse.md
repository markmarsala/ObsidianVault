PATH shows location of executables

```shell-session
echo $PATH
```

We can make the current directory the path
```shell-session
PATH=.:${PATH}
export PATH
echo $PATH
```

We can change ls to run in current directory instead of /bin/ls
```shell-session
touch ls
echo 'echo "PATH ABUSE!!"' > ls
chmod +x ls
```

