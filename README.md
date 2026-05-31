# Windows

Windows API bindings for [Rux](https://rux-lang.dev).

## Install

```toml
[Dependencies]
Windows = "0.2.0"
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

MIT
