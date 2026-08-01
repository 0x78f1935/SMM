# Installing FiveM mods

Some GTA V mods are packaged for **FiveM** (as FiveM "resources" - a folder
with an `fxmanifest.lua`). Simple Mods Manager can install the **asset** kind of
these into single-player: it recognises the resource, repackages its game assets
into a single-player add-on DLC pack, and writes the pack's `content.xml` /
`setup2.xml` for you automatically. Just add the resource folder (or its `.zip`)
the same way you add any other mod - SMM detects it and shows a **FiveM → SP**
badge on the Scan screen.

## What converts, and what doesn't

FiveM mods come in two flavours:

- **Asset mods** - add-on vehicles, maps/MLOs, ped/clothing, weapon models,
  texture packs. These are made of the same game files a normal single-player
  mod uses, so SMM can repackage them. **These are the ones SMM installs.**
- **Script mods** - gameplay resources that run Lua/JS code under the FiveM
  runtime (ESX, QBCore, NUI menus, and anything depending on them). There is no
  FiveM runtime in single-player, so **these can't be converted** - SMM tells you
  when a resource is script-only and installs nothing. A mod that's *both* (a car
  with a small helper script) installs the car and drops the script.

## The Enhanced (gen9) conversion - what comes through

FiveM assets are **Legacy** format. GTA V **Enhanced** uses the newer *gen9*
format, and the two aren't interchangeable. SMM automatically converts
**textures (`.ytd`)**, **drawables (`.ydr`)**, **drawable dictionaries
(`.ydd`)** and **fragments (`.yft` - the bodies of add-on vehicles)** to gen9;
collision bounds (`.ybn`), nav meshes (`.ynv`) and animation clips (`.ycd`)
work on both editions and install as they are. In practice:

- **Texture / graphics FiveM mods:** work.
- **Add-on props and map pieces:** their models and textures convert, so they
  come through (an unusual model that can't be converted is neutralised with a
  note rather than crashing the game).
- **Add-on vehicles:** convert and drive, now including **breakable glass,
  convertible tops, waving flags / awnings (cloth) and multi-part bodies**
  (drawable arrays). In the rare case a vehicle uses a mesh format SMM can't
  convert, it is left Legacy with a clear warning (prefer an Enhanced version
  when one exists).
- **Path data (`.ynd`):** not converted - a resource shipping it installs what
  it can and says so. Nav meshes (`.ynv`) and animation clips (`.ycd`) need no
  conversion and just work.

## How it looks

1. Add the FiveM resource folder (or `.zip`) like any other download.
2. On the Scan screen it shows a **FiveM → SP** badge, plus any notes (scripts
   dropped, models needing conversion, etc.).
3. Install into a profile as usual. The pack lands under
   `mods\update\x64\dlcpacks\<name>\` and is registered automatically.

If a converted add-on doesn't work for you, please
[open an issue](https://github.com/0x78f1935/SMM/issues) so the conversion can be
improved.
