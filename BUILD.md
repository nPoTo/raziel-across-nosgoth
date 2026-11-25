# Building Raziel Across Nosgoth R79

Download and extract `raziel-across-nosgoth-R79-source.zip` from this repository or the R79 release. This is the complete corresponding **mod** source, not either game's source or assets. Keep its directory tree unchanged.

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

`compile-args.json` contains the exact R79 native compiler arguments and feature defines. The archive includes the actual C inputs, local header closure, generated typed sheet records and authored policy JSON inputs. MinHook source and its license are included. Output is `workbench/sr1-local-controller-test/build/defiance-combat-r60/SR1LocalController.asi`.

The internal `r60`/`RazielR60` path names are runtime compatibility names. They are not the public build number; this source is R79. Do not rename them without updating the loader, provider and installation ownership paths together.

The native source closure was rebuilt from the staged R79 source. Rebuilding with another compiler or changed flags can produce different bytes. Existing executable/module fingerprints must be revalidated before supporting another game build; do not remove those checks to force compatibility.

## C# helpers

Sources are in `portable/`. Compile for **x64** using the .NET Framework C# compiler:

| Output | Source files | Target |
|---|---|---|
| `RazielBootstrap.exe` | `Bootstrap60.cs`, `PortableCore60.cs`, `UpdateCore79.cs`, `Version79.cs` | winexe |
| `RazielUpdater.exe` | `Updater79.cs`, `PortableCore60.cs`, `UpdateCore79.cs`, `Version79.cs` | winexe |
| `Uninstall R60.exe` | `Uninstall60.cs`, `InstallEngine60.cs`, `PortableCore60.cs` | winexe |
| `Raziel R79 Setup.exe` | `Setup60.cs`, `UpdateInstall79.cs`, `InstallEngine60.cs`, `PortableCore60.cs`, `Version79.cs` | winexe |

Reference `System.Windows.Forms`, `System.Drawing`, `System.Web.Extensions`, `System.Core`, `System.IO.Compression` and `System.IO.Compression.FileSystem`. From the extracted source root, run `./Build-Helpers.ps1`. It compiles the bootstrap and uninstaller, including the embedded Field Guide and tray icons. Supply `-PayloadZip <assembled-player-payload.zip>` to also build Setup. The source archive includes the English guide PNG, its preferred renderer source, authored cover/tray artwork and the helper manifest.

The bootstrap is an ordinary user application. Setup and uninstall use a standard application manifest requesting `requireAdministrator` for writes under Program Files. The setup executable embeds the assembled player payload with the resource name **payload.zip** (`/resource:<payload.zip>,payload.zip`).

## Runtime and player payload

`runtime/` contains the portable JavaScript modules/configuration. `runtime/portable-config60.mjs` provides the game-directory configuration from the helper's `RAZIEL_GAME_PATHS` value. Do not bake a developer's local paths into the package. Pose/blade/audio/HUD data are decoded from the installed games into RAM.

The R79 installer contains the full allowlisted payload: native ASI, rebuilt C# helpers, JavaScript runtime, permitted Node/ASI loader dependencies, guide, licenses and `SOURCE.zip`. To rebuild the installer, assemble these at the same relative paths, set `RazielR60/package.json` to version `R79` and regenerate its SHA-256 inventory for every payload file except the manifest itself. Update the runtime `host-manifest.json` with the rebuilt ASI hash and preserve the actual resource fingerprints. Embed that ZIP when compiling Setup. The installer verifies every listed payload entry before installation and detects unknown file conflicts.

`portable/package-r79.mjs` documents the source/header-closure and payload staging used for this build. It references the development workspace's resource-capture/build directories; it is **not** advertised as a one-command clean source build. Native compilation and all C# sources are supplied independently above. Node and Ultimate ASI Loader are separately licensed dependencies; use their official distributions and retain their complete notices. Do not obtain or redistribute original game assets as build inputs.

## What was checked

R79 passed typed-sheet preflight, native compilation, native source-closure rebuild, focused production damage/ownership/input/lifecycle tests, installer install/update/uninstall and failure-rollback fixtures, complete payload checksums, embedded-ZIP equality and a relocated RAM-only resource-provider check.

These checks do not certify full campaign compatibility or visual gameplay. The latest lighting appearance, full campaign coverage, a physical installer click-through and another clean physical PC remain gameplay/manual checks. The release is a public test build.



## Updater release contract

Include owned `RazielR60/RazielUpdater.exe` and `RazielR60/version.txt` in every player payload. Set `ReleaseVersion79.Number/Tag` and the package version consistently for a new release. The updater accepts published integer R tags, including public prereleases, only from nPoTo/raziel-across-nosgoth. Attach `Raziel R79 Setup.exe` directly to the GitHub release in addition to the installer ZIP and matching source ZIP. It requires the GitHub asset SHA-256 digest and exact versioned filename.

Startup checks are background/silent with no update or a connection failure. Download and install need separate Yes choices. Install Yes requests normal game closure without a second close question and waits for exit; no force-kill. Setup update mode waits for the old updater to exit, validates payload version before writes and installed version before DONE. Ordinary installer ownership/rollback rules are retained. A real future release-to-release upgrade and second physical PC remain unverified; isolated production transaction/verification tests passed.
