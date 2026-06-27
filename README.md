# Windows

Windows API bindings for Rux.

The `Windows` package provides direct access to selected Win32 APIs from `kernel32.dll`, including memory management, console I/O, file operations, process information, directory enumeration, string conversion, dynamic library loading, and system time functions.

All functions closely follow the official Microsoft Win32 API documentation.

## Installation

Add `Windows` to your `Rux.toml`:

```toml
[Dependencies]
Windows = "*"
```

## Features

| Category          | APIs                                                                           |
| ----------------- | ------------------------------------------------------------------------------ |
| Memory Management | HeapAlloc, HeapReAlloc, HeapFree, GetProcessHeap                               |
| Memory Operations | RtlCopyMemory, RtlFillMemory, RtlZeroMemory, RtlCompareMemory                  |
| Console I/O       | AllocConsole, GetStdHandle, ReadConsoleA, WriteConsoleA, WriteConsoleW, Beep   |
| String Conversion | MultiByteToWideChar, WideCharToMultiByte                                       |
| Process & Thread  | ExitProcess, Sleep, GetCurrentProcessId, GetCurrentThreadId                    |
| System Time       | GetTickCount64, GetLocalTime, GetSystemTime                                    |
| File I/O          | CreateFileA, ReadFile, WriteFile, GetFileSizeEx, SetFilePointerEx, CloseHandle |
| Filesystem        | DeleteFileA, CopyFileA, MoveFileA, CreateDirectoryA, RemoveDirectoryA          |
| Directories       | GetCurrentDirectoryA, SetCurrentDirectoryA                                     |
| File Enumeration  | FindFirstFileA, FindNextFileA, FindClose                                       |
| Dynamic Libraries | LoadLibraryA, FreeLibrary, GetProcAddress                                      |
| Error Handling    | GetLastError                                                                   |

## Types

The package also exposes several Win32 structures and enums:

### Structures

* FileTime
* SystemTime
* Win32FindDataA

### Enums

* StdHandle
* CodePage
* FileAccess
* FileShare
* CreationDisposition
* FileAttributes
* SeekOrigin

### Constants

* INVALID_HANDLE_VALUE

## Naming Convention

Functions ending with `A` use ANSI (char8) strings.

Functions ending with `W` use UTF-16 (char16) strings.

Examples:

* `CreateFileA`
* `WriteConsoleA`
* `WriteConsoleW`

---

# Example: Console Output

```rux
import Windows::*;

func Main() -> int {
    AllocConsole();

    let stdout = GetStdHandle(StdHandle.Output);

    let message = "Hello from Win32!\n";

    let written: uint32 = 0;

    WriteConsoleA(
        stdout,
        message.data,
        message.length as uint32,
        &written,
        null
    );

    return 0;
}
```

---

# Example: Heap Allocation

```rux
import Windows::*;

func Main() -> int {
    let heap = GetProcessHeap();

    let buffer = HeapAlloc(heap, 0, 1024);

    if (buffer == null)
        return 1;

    RtlZeroMemory(buffer, 1024);

    HeapFree(heap, 0, buffer);

    return 0;
}
```

---

# Example: Create and Write a File

```rux
import Windows::*;

func Main() -> int {
    let file = CreateFileA(
        "example.txt".data,
        FileAccess.Write,
        FileShare.None,
        null,
        CreationDisposition.CreateAlways,
        FileAttributes.Normal,
        null
    );

    if (file == INVALID_HANDLE_VALUE)
        return 1;

    let text = "Hello, Windows File API!\n";

    let written: uint32 = 0;

    WriteFile(
        file,
        text.data,
        text.length as uint32,
        &written,
        null
    );

    CloseHandle(file);

    return 0;
}
```

---

# Example: Load a DLL

```rux
import Windows::*;

func Main() -> int {
    let kernel32 = LoadLibraryA("kernel32.dll".data);

    if (kernel32 == null)
        return 1;

    let proc = GetProcAddress(
        kernel32,
        "GetTickCount64".data
    );

    FreeLibrary(kernel32);

    return 0;
}
```

## License

[MIT](LICENSE)
