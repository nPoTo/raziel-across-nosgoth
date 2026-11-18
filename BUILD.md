# Building the Windows v0.2 preview

## Requirements

- 64-bit Windows with PowerShell 7, Windows PowerShell 5.1, and the .NET Framework 4.x C# compiler.
- Node.js 24.22 or later.
- The x64 UCRT LLVM-MinGW toolchain used for this build. Download the [20261006 release](https://github.com/mstorsjo/llvm-mingw/releases/tag/20261006) and locate `bin/x86_64-w64-mingw32-clang.exe` inside it.
- The original v0.2 setup EXE for the installer repack. Its SHA-256 is `B1E24604642BE44AD6D81A5299F8AA7C9A36645F217AD19331C0DFB4A5E6A311`.

Extract the source archive into a short local directory and run these commands from that directory. Adjust the toolchain and installer paths to match your downloads.

## 1. Compile the native modules

```powershell
$clang = 'C:\tools\llvm-mingw\bin\x86_64-w64-mingw32-clang.exe'
node .\release\build-native.mjs $clang .\build-output\native
```

This builds MinHook from the included source, then compiles `LOKProbe.asi`, `quick-world-observer.exe`, `Chapter10AutoLoad.asi`, and `Chapter10SpawnPortal.asi` using the recorded compiler arguments and generated headers. The observer is rebuilt with the new probe hash. The results are in `build-output/native/native/`.

## 2. Build the installer

```powershell
.\release\Build-SetupFromPreview.ps1 `
  -PreviewInstaller 'C:\downloads\Raziel Across Nosgoth Setup.exe' `
  -NativeOutput .\build-output\native `
  -OutputDirectory .\build-output\setup `
  -Version '0.2'
```

The packaging script verifies the input EXE, extracts its embedded runtime and replaces the four rebuilt binaries and runtime source. It reuses the remaining pinned helper executables, Node.js, the ASI loader, the guide art, and the companion save from the original release. It updates the runtime digests, runs the offline runner check, and builds the self-contained EXE from the included C# and PowerShell installer sources. The EXE appears in `build-output/setup/installer/`.

This preview rebuild reuses the release dependencies listed above. It does not reconstruct every auxiliary helper binary from a blank checkout. A rebuilt installer has different hashes from the original download.

## 3. Validate the build

Check that the offline runner reports `ready: true` and a verified runtime. For a live test, install on a Windows machine with supported builds of both remastered games. Open the new desktop shortcut and confirm that Defiance loads the companion save, SR1 starts, and the first **F10** enables the mod once Raziel can move. Close both games and use the Windows Apps entry to test uninstall.

## Third-party components

- [MinHook](https://github.com/TsudaKageyu/minhook) is included under its own license at `dependencies/minhook/LICENSE.txt` in the source archive.
- [Ultimate ASI Loader v9.7.4](https://github.com/ThirteenAG/Ultimate-ASI-Loader/releases/tag/v9.7.4) is reused from the release. Its notice is at `workbench/raziel-windows-setup/ASI-LOADER-LICENSE.txt` in the source archive.
- [Node.js 24.22.1](https://nodejs.org/download/release/v24.22.1/) is reused as a separate runtime.
