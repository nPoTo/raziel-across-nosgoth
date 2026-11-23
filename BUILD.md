# Building Raziel Across Nosgoth R73

Download and extract `raziel-across-nosgoth-R73-source.zip` from this repository or the R73 release. This is the complete corresponding **mod** source, not either game's source or assets. Keep its directory tree unchanged.

## Prerequisites

- Windows x64.
- An x64 **llvm-mingw Clang/UCRT** toolchain. The native build was checked with llvm-mingw `20261006`.
- Windows .NET Framework C# compiler (`Framework64/v4.0.30319/csc.exe`) and standard framework assemblies.
- Node.js 24+ for the JavaScript resource provider. The player package includes its runtime.
- Supported, separately installed Soul Reaver and Defiance Remastered for resource decoding/gameplay. The source archive contains no game archives or decoded resources.

## Native module

From the extracted source root, run:

```powershell
& .\Build-Native.ps1 -Compiler 'C:\path\to\x86_64-w64-mingw32-clang.exe'
```

`compile-args.json` contains the exact R73 native compiler arguments and feature defines. The archive includes the actual C inputs, local header closure, generated typed sheet records and authored policy JSON inputs. MinHook source and its license are included. Output is `workbench/sr1-local-controller-test/build/defiance-combat-r60/SR1LocalController.asi`.

The internal `r60`/`RazielR60` path names are runtime compatibility names. They are not the public build number; this source is R73. Do not rename them without updating the loader, provider and installation ownership paths together.

The native source closure was rebuilt from the staged R73 source. Rebuilding with another compiler or changed flags can produce different bytes. Existing executable/module fingerprints must be revalidated before supporting another game build; do not remove those checks to force compatibility.

## C# helpers

Sources are in `portable/`. Compile for **x64** using the .NET Framework C# compiler:

| Output | Source files | Target |
|---|---|---|
| `RazielBootstrap.exe` | `Bootstrap60.cs`, `PortableCore60.cs` | winexe |
| `Uninstall R60.exe` | `Uninstall60.cs`, `InstallEngine60.cs`, `PortableCore60.cs` | winexe |
| `Raziel R73 Setup.exe` | `Setup60.cs`, `InstallEngine60.cs`, `PortableCore60.cs` | winexe |

Reference `System.Windows.Forms`, `System.Drawing`, `System.Web.Extensions`, `System.Core`, `System.IO.Compression` and `System.IO.Compression.FileSystem`. From the extracted source root, run `./Build-Helpers.ps1`. It compiles the bootstrap and uninstaller, including the embedded Field Guide and tray icons. Supply `-PayloadZip <assembled-player-payload.zip>` to also build Setup. The source archive includes the English guide PNG, its preferred renderer source, authored cover/tray artwork and the helper manifest.

The bootstrap is an ordinary user application. Setup and uninstall use a standard application manifest requesting `requireAdministrator` for writes under Program Files. The setup executable embeds the assembled player payload with the resource name **payload.zip** (`/resource:<payload.zip>,payload.zip`).

## Runtime and player payload

`runtime/` contains the portable JavaScript modules/configuration. `runtime/portable-config60.mjs` provides the game-directory configuration from the helper's `RAZIEL_GAME_PATHS` value. Do not bake a developer's local paths into the package. Pose/blade/audio/HUD data are decoded from the installed games into RAM.

The R73 installer contains the full allowlisted payload: native ASI, rebuilt C# helpers, JavaScript runtime, permitted Node/ASI loader dependencies, guide, licenses and `SOURCE.zip`. To rebuild the installer, assemble these at the same relative paths, set `RazielR60/package.json` to version `R73` and regenerate its SHA-256 inventory for every payload file except the manifest itself. Update the runtime `host-manifest.json` with the rebuilt ASI hash and preserve the actual resource fingerprints. Embed that ZIP when compiling Setup. The installer verifies every listed payload entry before installation and detects unknown file conflicts.

`portable/package-r73.mjs` documents the source/header-closure and payload staging used for this build. It references the development workspace's resource-capture/build directories; it is **not** advertised as a one-command clean source build. Native compilation and all C# sources are supplied independently above. Node and Ultimate ASI Loader are separately licensed dependencies; use their official distributions and retain their complete notices. Do not obtain or redistribute original game assets as build inputs.

## What was checked

R73 passed typed-sheet preflight, native compilation, native source-closure rebuild, focused production damage/ownership/input/lifecycle tests, installer install/update/uninstall and failure-rollback fixtures, complete payload checksums, embedded-ZIP equality and a relocated RAM-only resource-provider check.

These checks do not certify full campaign compatibility or visual gameplay. The latest lighting appearance, full campaign coverage, a physical installer click-through and another clean physical PC remain gameplay/manual checks. The release is a public test build.


