# Building R116

Extract the attached corresponding-source ZIP. Use x64 llvm-mingw Clang/UCRT
(checked toolchain 20261006), Node.js 24+, Windows PowerShell and .NET Framework 4.
Run Build-Native.ps1 -Compiler <path to x86_64-w64-mingw32-clang.exe>.
The exact arguments, C source/header closure, generated records and authored
typed policies are included. Output is workbench/defiance-kain-r1/build/gameplay80/SR1LocalController.asi.

Run Build-Helpers.ps1 to rebuild the bootstrap, updater and uninstaller.
Add -PayloadZip <player payload ZIP> to also rebuild Raziel R116 Setup.exe.
The player payload uses the existing RazielR60 directory layout. Include
RazielUpdater.exe, version.txt containing R116, runtime/, SOURCE.zip, the loader,
node.exe, GUIDE.txt and the supplied dependency licenses. Regenerate the SHA256
inventory in RazielR60/package.json (version R116) for every file except package.json.
The Setup embeds this ZIP as payload.zip. The installer uses original owned-file
backup/rollback logic for updates from R79.

runtime/ contains the resource provider, including serve-release116.mjs.
The bootstrap passes the selected game paths through RAZIEL_GAME_PATHS and
writes the selected Defiance executable as UTF16 in the per-run data directory.
The native worker verifies its pinned SHA256 before mapping original AI code.
Both resource transfers stay in RAM. Neither game archive is part of the source.

Publish tag R116 with the direct asset Raziel.R116.Setup.exe, installer ZIP,
matching source ZIP and SHA256SUMS.txt. The corrected public R79 updater accepts
this exact dotted name and the GitHub asset SHA256. Keep the repository and
versioned URL unchanged for updater compatibility.

The source includes the packaging preparation/build scripts under release-tools/.
Start from an allowlisted R79 payload layout or assemble the paths above with
the supplied runtime and rebuilt binaries. Tests exercise isolated install/update,
uninstall, updater selection and both resource packets. The creator will test
the actual R79 to R116 GUI update on another PC.
