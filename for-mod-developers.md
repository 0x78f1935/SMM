# SMM support for mod developers

Simple Mods Manager already works out of the box for most mods: it reads your
readme, recognises `scripts/`, `.asi` plugins, `mods\` overlays and `.rpf`
packs, and installs everything to the right place. But if your mod has an
unusual layout - or you just want **guaranteed** correct installation without
relying on SMM's guesses - you can ship a tiny manifest that tells SMM exactly
where each file goes.

## Add an `SMM.json` to your archive

Drop a file called **`SMM.json`** (or `smm.json` - the name is
case-insensitive) anywhere in your downloadable archive; the archive root is
the natural place (and required if you use the `dependencies` key - see
below). It's a plain JSON object that maps **a file in your archive** to
**where it should be installed**:

```json
{
  "MyTrainer.asi": "MyTrainer.asi",
  "config/MyTrainer.ini": "scripts/MyTrainer.ini",
  "textures/vehshare.ytd": "mods/update/update.rpf/x64/textures/vehshare.ytd",
  "MyPack/dlc.rpf": "mods/update/x64/dlcpacks/mypack/dlc.rpf"
}
```

- **The key** is the path to one of your files, relative to the folder the
  `SMM.json` sits in (forward slashes). So an `SMM.json` at the archive root
  uses paths as they appear in the archive.
- **The value** is the install target, relative to the **GTA V game folder**,
  written in Simple Mods Loader form:
    - a game-root file - `dinput8.dll`, `MyTrainer.asi`;
    - a script or its config - `scripts/MyTrainer.dll`, `scripts/MyTrainer.ini`;
    - content **inside** an archive - `mods/update/update.rpf/<path inside the rpf>`;
    - a whole add-on pack - `mods/update/x64/dlcpacks/<yourpack>/dlc.rpf`.

## How SMM uses it

When an `SMM.json` is present, SMM **still** runs its normal detection first
(your readme, the folder layout, the compatibility checks) - but then it
**validates the result against your `SMM.json`, and your manifest is the source
of truth**. For every file you list, SMM:

- installs it exactly where you said, overriding whatever it would have guessed;
- installs it even if SMM would normally have skipped it (for example a loose
  `.ini`, or a `.pdb` you deliberately want shipped);
- leaves the `SMM.json` file itself out of the install.

Files you **don't** list keep SMM's automatic mapping, so you only need to name
the ones you care about. If you list a file that isn't in the archive, SMM notes
it and moves on.

## Declaring iFruitAddon2 phone contacts

If your mod adds contacts to the in-game phone through **iFruitAddon2**, tell SMM
how many with an **`ifruitContacts`** field. SMM keeps iFruitAddon2's `StartIndex`
in step with the total across installed mods (starting at 0) so players never see a
run of empty contact entries - no manual `config.ini` editing. Your declared number
is authoritative; without it, SMM falls back to guessing from your readme, so
declaring it is the reliable way.

It's just another key in the same `SMM.json` (it sits alongside your file map - SMM
ignores it when placing files):

```json
{
  "MyPhoneMod.dll": "scripts/MyPhoneMod.dll",
  "ifruitContacts": 3
}
```

**Multi-mod bundles.** If one archive ships several contact mods, either give each
sub-mod folder its own `SMM.json` (the counts add up), or list them in one file and
SMM sums them:

```json
{
  "mods": [
    { "ifruitContacts": 2 },
    { "ifruitContacts": 3 }
  ]
}
```

Set `"ifruitContacts": 0` to say **explicitly** that your mod adds no contacts (this
suppresses the readme guess).

## Declaring dependencies

If your mod needs **other mods** installed alongside it, list them under a
**`dependencies`** key so SMM can offer to install them in the same step. Each entry is
a gta5-mods.com mod-page URL, with an optional display `name` and `required` flag
(defaults to `true` - pre-ticked in the prompt):

```json
{
  "MyMap/dlc.rpf": "mods/update/x64/dlcpacks/mymap/dlc.rpf",
  "dependencies": [
    { "url": "https://www.gta5-mods.com/misc/swimmable-water-everywhere", "name": "Swimmable Water Everywhere", "required": true },
    { "url": "https://www.gta5-mods.com/maps/my-scenario", "required": false }
  ]
}
```

A bare URL string works too (`"dependencies": ["https://www.gta5-mods.com/..."]`). When a
player installs your mod, SMM shows a checklist of these (merged with any gta5-mods.com
links it finds in your page description) and installs the ones they tick.

- **Only gta5-mods.com** mod pages are installable this way; other links are ignored.
- The `dependencies` key is only read from an `SMM.json` at the **archive
  root** - in a nested folder it still maps files, but its dependency list is
  not picked up.
- SMM **never** installs ScriptHookV or an ASI loader as a dependency, even if you
  list one - it provides and manages those itself (its loader is `hid.dll` and its
  handler fills `ScriptHookV.dll`). Such a link is dropped from the offer, and any file a
  dependency would write to one of those managed slots is skipped on install.

## Two rules SMM won't let you break

- **The loader is off-limits.** You can't map a file onto `hid.dll` (or the
  legacy `dsound.dll`) - that's the Simple Mods Loader itself, and SMM manages
  it. A few other proxy names (`version.dll`, `winmm.dll`) are reserved for
  the same reason. Any such entry is ignored.
- **Paths stay inside the archive.** Keys that try to escape the archive
  (`../…`) are ignored.

That's the whole feature: one small file, and your mod installs perfectly for
every SMM user with no guesswork.
