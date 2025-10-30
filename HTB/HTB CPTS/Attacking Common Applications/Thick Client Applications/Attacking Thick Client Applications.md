## Penetration Testing Steps

**Information Gathering
- CFF Explorer: https://ntcore.com/?page_id=388
- Detect It Easy: https://github.com/horsicq/Detect-It-Easy
- Process Monitor: https://learn.microsoft.com/en-us/sysinternals/downloads/procmon
- Strings: https://learn.microsoft.com/en-us/sysinternals/downloads/strings

**Client Side Attacks
- Ghidra: https://www.ghidra-sre.org/
- dnSpy: https://github.com/dnSpy/dnSpy
- IDA: https://hex-rays.com/ida-pro/
- x64dbg: https://x64dbg.com/
- OllyDbg: http://www.ollydbg.de/
- JADX: https://github.com/skylot/jadx
- Radare2: https://www.radare.org/r/index.html
- Frida: https://frida.re/

**Network Side Attacks
- Wireshark: https://www.wireshark.org/
- tcpdump: https://www.tcpdump.org/
- TCPView: https://learn.microsoft.com/en-us/sysinternals/downloads/tcpview
- Burp Suite: https://portswigger.net/burp


## Retrieving hardcoded Credentials from Thick-Client Applications

1. Find application on exposed smb server (Restart-OracleService.exe)
2. Put into ProcMon64 from Sysinternals to show it creates a dump
3. Set permissions on temp file to uncheck 'delete subfolders and files' and 'delete'
4. Remove deletion of batch files
5. Put restart-service.exe through x64dbg and uncheck everything but exit breakpoint
6. Follow memory map
7. Double-click on 3000 MAP -RW--
8. Right click and slelect dump memory to file
9. Run strings on the exported file
10. Use De4Dot to reverse .NET executable by dragging the file onto De4Dot
11. Drag and drop this into DnSpy

