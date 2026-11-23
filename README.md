# Raziel Across Nosgoth — R69

A Windows mod bringing Defiance movement, ground and aerial combat animations, directional dodges, telekinesis, soul-draining finishers and charged Reaver abilities into **Soul Reaver 1 Remastered**. Play the Soul Reaver 1 campaign with your own save. This does not modify the Soul Reaver 2 campaign.

**Public test build.** Full campaign, every enemy/boss/puzzle and a second clean physical PC are not fully verified. The latest door, lever, cover and bell changes still need full gameplay confirmation. If you get stuck, switch the mod Off with F10, complete the action, then switch it On again and report what happened.

## Download and install

1. Install **Legacy of Kain: Soul Reaver 1 & 2 Remastered** and **Legacy of Kain: Defiance Remastered** on Windows 10/11 x64. Both games must work independently. The original non-remastered releases are not supported.
2. Close both games and back up your saves. [Download the R69 installer ZIP](https://github.com/nPoTo/raziel-across-nosgoth/releases/download/R69/Raziel-R69-Installer.zip).
3. Extract the ZIP, read the included README and run **Raziel R69 Setup.exe**. Choose the SR1 folder containing `SRX.exe` and `1/sr1.dll`, then the Defiance folder containing `defiance.exe` and `bigfile.x64.dat`. Follow **Next → Install → Finish**.
4. Start Soul Reaver 1 normally from Steam, its original executable or your existing shortcut. **Defiance must remain installed, but does not need to run.** No companion save or separate launcher is needed.
5. Load your save and press **F10**. First-time resource preparation can take about a minute or more; F10 can queue activation. The tray icon shows Loading, On or Off. Later F10 presses toggle the mod without preparing the resources again. **F7** opens the complete guide.

Node is bundled; no Python, compiler or development tools are needed to play. The Windows helper uses .NET Framework. Game resources are read from the installed games into RAM; original game archives and saves are not distributed.

**R69 update:** Doors, levers, valves and covers now get the original game controls before attack controls. Fixed a check that could stop a normal ground strike from reaching the Cathedral bell. The arena and enemy-summoning shortcuts have been removed. These interaction changes still need full gameplay confirmation.

## Controls

| Action | Keyboard / mouse | Xbox controller |
|---|---|---|
| Move | WASD | Left stick |
| Normal attack / combo | Left click | X |
| Ground lift / airborne slam | Right click | Y |
| Jump | Space | A |
| Target lock | Hold Shift | Hold LB |
| Directional dodge | Shift + Space + direction | LB + A + direction |
| Telekinesis | F | B |
| Feed / finish eligible stunned enemy | E | RB |
| Cycle unlocked swords | Mouse wheel, Z / X | D-pad Left / Right |
| Select unlocked sword | 1–8, including numpad | — |
| Glyph menu / map | C / M | Down then A for realm selection |
| Toggle mod / guide | F10 / F7 | Use keyboard hotkeys |

Realm shifting still follows the game's requirements, including a usable portal when returning to Material. Carried physical weapons/items, water, gliding, story scenes and menus retain native SR1 rules.

## Combos and abilities

- **Ground chain:** press normal attack repeatedly to link strikes.
- **Thrust series:** enter a new direction and immediately press normal attack, then continue attacking. Release/re-enter a long-held direction to create a new directional input.
- **Launcher:** hold right click / Y on the ground. A quick tap and a hold have different actions.
- **Aerial chain:** jump and use normal attacks.
- **Helicopter:** while airborne, enter a new direction and immediately use normal attack. Further presses fund two rotations each, up to ten. Each actual enemy contact deals the selected sword's normal strike damage; a missed rotation does not damage anything.
- **Air slam:** right click / Y in the air. The directional special variant adds a rebound.
- **Finisher:** approach an eligible stunned enemy and press E / RB. No extra ground strike should be needed after airborne exhaustion.
- **Charged spell:** fill the sword meter through real hits, then hold normal attack for about one second. The release spends the full meter. Enemies actually damaged by a charged spell cannot regenerate for eight seconds.

## Reavers and progression

Acquire the base Soul Reaver in your current save first. Losing health does not revoke an acquired Reaver. The blade appears during combat or target lock. Cycling skips locked forms; loading another save uses that save's own unlocks.

| Number | Form | Unlock |
|---|---|---|
| 1 | Normal / Spectral | Base Reaver; form follows the current realm |
| 2 | Dark | Force Glyph |
| 3 | Earth | Stone Glyph |
| 4 | Water | Water Glyph |
| 5 | Wind | Sound Glyph |
| 6 | Fire | Fire Glyph |
| 7 | Light | Sunlight Glyph |
| 8 | Spirit | All Glyphs including Shift; Material only |

Standard Reaver strikes deal **500 + 300 per acquired elemental form**. Spirit deals **5,000**. Helicopter contacts use the same sword damage. Native enemy vulnerabilities and boss conditions still apply. The sword meter and full-charge effects show charge; there is no HP/DMG test popup.

## Optional debug shortcuts

**F6** grants the base Reaver. **F9** toggles the all-powers/Glyphs/portals/health/mana preset. Pressing F9 again restores the captured state in the same session, but saving the game can make changes persist. Back up saves before using these shortcuts. The original Debug menu opens with **F10 Off, then E + left click / B + X**. These shortcuts are not required for ordinary play.

## Help and uninstall

If Loading is shown, wait. If Off is shown, focus SR1 and press F10. Use the **Choose Defiance folder** button in the F7 guide if the installation was not detected; restart SR1 after correcting a resource-loading failure. A missing sword may simply be locked in the current save or realm.

For an installer installation, close both games and run `RazielR60/Uninstall R60.exe` inside the SR1 folder. It restores backed-up owned files and preserves saves/user-modified files. `RazielR60` is the package's internal folder name: keep its spelling unchanged. Logs are under `%LOCALAPPDATA%/RazielAcrossNosgoth/R60/run-*`.

See [README.txt](README.txt) for the complete installation and field guide, and [BUILD.md](BUILD.md) for source/rebuild instructions.

## Source, licensing and credits

The complete corresponding R69 mod source is supplied as [raziel-across-nosgoth-R69-source.zip](raziel-across-nosgoth-R69-source.zip), and is also embedded in the installer payload as `RazielR60/SOURCE.zip`. It contains the native compiler inputs/header closure, authored typed policy inputs, runtime JavaScript and C# bootstrap/installer/uninstaller sources. The versioned archive is the source distribution in this repository.

The mod is **GPL-3.0-or-later**; see [LICENSE](LICENSE). Dependencies include MinHook (BSD-style), Ultimate ASI Loader (MIT) and Node.js with its dependency notices. Their licenses/attributions are included. Original game resources and trademarks belong to their respective owners. No game archives, saves, credentials or gameplay captures are included.

Shortcut feedback: F6 and F9 require focused, ordinary grounded gameplay. Requests wait up to five seconds for a safe frame. F6 grants the native base-Reaver acquisition flags. F9 temporarily suspends mod hooks before changing powers and resumes the previous activation choice. The last result is shown at the top of F7 and in a tray notification. Changes may persist if the game saves.

Original Debug menu: turn the mod Off with F10, release the buttons, then press E + left click on keyboard/mouse or B + X on controller. Use the same combination to close it, then F10 to resume. There is no added Pause → Debug row. The original menu includes Levels, Raziel, Goodies, Camera and AI/Script options. Debug changes can alter progress; back up your saves.

