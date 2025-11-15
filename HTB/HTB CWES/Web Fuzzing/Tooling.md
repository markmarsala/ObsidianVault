## Essential Concepts
- Wordlist
- Payload
- Response Analysis
- Fuzzer
- False Positive
- False Negative
- Fuzzing Scope

```
sudo apt update
sudo apt install -y golang
sudo apt install -y python3 python3-pip
sudo apt install pipx
pipx ensurepath
sudo pipx ensurepath --global
go version
python3 --version
go install github.com/ffuf/ffuf/v2@latest
go install github.com/OJ/gobuster/v3@latest
curl -sL https://raw.githubusercontent.com/epi052/feroxbuster/main/install-nix.sh | sudo bash -s $HOME/.local/bin
pipx install git+https://github.com/WebFuzzForge/wenum
pipx runpip wenum install setuptools
```


## Tools
- ffuf
- gobuster
- feroxbuster
- wfuzz/wenum
