## ELF Executable Examination

There is a binary called octopus_checker.

We can debug it with PEDA: https://github.com/longld/peda

```
gdb ./octopus_checker
set disassembly-flavor intel
run
disas main
```
- We find    0x0000555555555607 <+433>:	call   0x5555555551b0 (SQLDriverConnect@plt)
- Let's add a breakpoint here.

```
b *0x5555555551b0
run
```
- We find credentials


## DLL File Examination

We find MultimasterAPI.dll binary.

```
Get-FileMetaData .\MultimasterAPI.dll
```
- .NET assembly

We can debug it with dnSpy: https://github.com/0xd4d/dnSpy
- MultimasterAPI.Controllers -> ColleagueController reveals credentials
