# Windows

Windows API bindings for [Rux](https://rux-lang.dev).All functions follow the official [Microsoft Win32 documentation](https://learn.microsoft.com/en-us/windows/win32/api/).

## Install

```toml
[Dependencies]
Windows = "0.2.1"
```

## Modules

| File | What |
|------|------|
| `Kernel32.rux` | kernel32.dll — processes, memory, threads, DLLs, sync, console, heap |
| `Pe.rux` | PE/COFF structs (DOS header, NT headers, IAT, EAT, sections) |
| `Process.rux` | Process enumeration, DLL injection, memory read/write |
| `Hook.rux` | IAT and inline (trampoline) function hooking |
| `ProxyDll.rux` | Proxy DLL pattern — forwarding to the real DLL |
| `Advapi32.rux` | advapi32.dll — registry, services, security, LSA |
| `DbgHelp.rux` | dbghelp.dll — minidumps, stack walking, symbols |
| `Gdi32.rux` | gdi32.dll — bitmap, font, DC, region |
| `NtDll.rux` | ntdll.dll — NT API (process, system, memory info) |
| `Psapi.rux` | psapi.dll — process/device driver enumeration |
| `User32.rux` | user32.dll — windows, messages, controls, input |
| `WinHvPlatform.rux` | WinHvPlatform.dll — Hyper-V whpx |
| `Ws2_32.rux` | ws2_32.dll — sockets (Winsock 2) |

### Memory Management

Functions for heap-based memory allocation.

| Function           | Description                                                 |
| ------------------ | ----------------------------------------------------------- |
| `GetProcessHeap()` | Returns a handle to the default heap of the calling process |
| `HeapAlloc()`      | Allocates a block of memory from a specified heap           |
| `HeapReAlloc()`    | Reallocates a block of memory from a heap                   |
| `HeapFree()`       | Frees a memory block allocated from a heap                  |

### Memory Operations

Low-level memory utility routines (RTL).

| Function          | Description                                    |
| ----------------- | ---------------------------------------------- |
| `RtlFillMemory()` | Fills a block of memory with a specified value |
| `RtlZeroMemory()` | Fills a block of memory with zeros             |
| `RtlCopyMemory()` | Copies a block of memory to another location   |

### Console I/O

Functions for allocating and writing to a console.

| Function          | Description                                                     |
| ----------------- | --------------------------------------------------------------- |
| `AllocConsole()`  | Allocates a new console for the calling process                 |
| `GetStdHandle()`  | Retrieves a handle to a standard device (stdin, stdout, stderr) |
| `WriteConsoleA()` | Writes a string of ANSI characters to the console               |
| `WriteConsoleW()` | Writes a string of UTF-16 characters to the console             |
| `Beep()`          | Generates a tone on the system speaker                          |

### String Conversion

| Function                | Description                                               |
| ----------------------- | --------------------------------------------------------- |
| `MultiByteToWideChar()` | Converts a multi-byte ANSI string to a UTF-16 wide string |

### Process & Thread Control

| Function        | Description                                        |
| --------------- | -------------------------------------------------- |
| `Sleep()`       | Suspends the execution of the current thread       |
| `ExitProcess()` | Terminates the calling process and all its threads |

### Error Handling

| Function         | Description                                         |
| ---------------- | --------------------------------------------------- |
| `GetLastError()` | Retrieves the last-error code of the calling thread |


## Example

```rux
import Windows::*;

func Main() -> int {
    let heap = GetProcessHeap();
    let mem  = HeapAlloc(heap, 0u32, 1024u);
    RtlZeroMemory(mem, 1024u);

    let stdout = GetStdHandle(StdHandle::Output as uint32);
    var written: uint32 = 0u32;
    WriteConsoleA(stdout, "Hello, Windows!\n\0", 16u32, &written, null);

    HeapFree(heap, 0u32, mem);
    return 0;
}
```

## License

[MIT](LICENSE)
