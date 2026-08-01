# Hard dependencies - automatic

Every profile needs a few things to run mods, so Simple Mods Manager installs
the latest versions automatically when you create a fresh profile:

- **SMM Loader** - Simple Mods Manager's **own** mod loader, shipped inside
  the app itself (nothing is downloaded for it). Once it has written its settings
  file, **SmmLoader.ini** is editable right on the SMM Loader card (the same way
  **SMH.ini** appears on the SMM Handler card).
- **ScriptHookV (the script handler)** - the library that gives script mods
  access to the game's functions; every `.asi` script plugin needs it. By
  default the slot is filled by **SMM Handler (SMH)** - Simple Mods Manager's
  own script handler, shipped inside the app (nothing is downloaded; the file is
  installed as `ScriptHookV.dll` because that is the name mods look for). A
  switch on the card installs **Alexander Blade's original ScriptHookV** from
  dev-c.com instead if you prefer it; its bundled *NativeTrainer.asi* (a
  sample trainer) is **not** installed - add it yourself if you want it.
  Neither is the extra ASI-loader DLL it ships
  (`dinput8.dll` / `xinput1_4.dll`): the SMM Loader loads your `.asi` plugins
  itself (see [Who loads your .asi plugins](#who-loads-your-asi-plugins)).
- **ScriptHookVDotNet** - the **Script Hook V .NET Enhanced** build by
  Chiheb-Bacha, from gta5-mods.com (made for GTA V Enhanced). It also has a
  **Nightly** button (see below).
- **iFruitAddon2** - the phone/contact library by **Bob74**, from its GitHub
  releases; installs into `scripts\` (many phone mods depend on it). SMM keeps its
  contact **StartIndex** in step automatically: it starts at 0 and adds up the
  contacts each installed mod declares (read from the mod's readme/description), so
  the in-game phone shows no run of empty contact entries. The setting stays editable
  on the iFruitAddon2 card if you want to override it.
- **NativeUI** - the classic menu library by **Guad**, from its GitHub
  releases; installs into `scripts\` (many script mods build their menus on it).
- **LemonUI** - NativeUI's modern successor by **justalemon**, from its GitHub
  releases; installs into `scripts\`. Most current script mods build their menus
  and on-screen displays on it. LemonUI ships a separate build per platform and
  Simple Mods Manager installs the **SHVDN3** one, which is the build single-player
  scripts use. It is installed for every profile because mods frequently *need* it
  without saying so anywhere in their download - one that doesn't mention it will
  simply misbehave, which is a miserable thing to debug.

Every profile also gets the two folders mods live in - `scripts\` (script mods and
their settings) and `mods\` (everything that replaces or adds game content) -
created for you. You never need to make them yourself.

**Update checks are polite by design.** Simple Mods Manager asks the mod sites what the
latest versions are **once, in the background, when it starts** - for the dependencies
*and* for every mod you installed via Browse - and remembers the answers in its data
folder for six hours. Opening a profile, or switching between profiles, then costs
**nothing**: those screens read the saved answers and never call out. Outdated rows still
get an Update button, and a row that hasn't been checked yet says so rather than pretending
to be current. Press **🔄 Updates** in a profile whenever you want a fresh look
right now.

Mods installed from **Nexus Mods** are checked through the official Nexus API, which only
answers with your account's API key - so they need your Nexus account **connected in
Settings**. Without it, their rows say exactly that instead of staying silent. Mods SMM
picked up as plain files (from your Downloads folder, with no store page recorded) have
nowhere to ask for a newer version, so they never show an Update button.

Opening a profile also **verifies each dependency's key files are actually
present**. If one is incomplete - for example ScriptHookV without its
`ScriptHookV.dll` - the row shows an amber ⚠ with the missing file. If it's also
outdated, its button reads **Update & repair**.

Hard dependencies can always be **reinstalled** (a Reinstall button fetches a
fresh copy, whether or not anything looks wrong) but can **never be removed** -
they're managed for you, and removing one would break the profile. Only regular
mods you install have a Remove button.

On the **active** profile, each dependency's card also shows its own **runtime
log** when one exists - `SmmLoader.log` on the loader card, `SmmHandler.log`
(or `ScriptHookV.log`) on the script-host card, `ScriptHookVDotNet.log` on the
.NET host card and `iFruitAddon2.log` on the phone-library card - with its size,
when it was last written, and a **View** button that opens the log right there.
That's the first place to look when something didn't load: the log tells you
what that component saw during the last game session.

## Who loads your .asi plugins

`.asi` plugins - trainers, menus, most script mods - have to be *loaded* into the
game by something. Traditionally that was an extra DLL bundled with ScriptHookV
(`dinput8.dll`). **The SMM Loader does that job itself**, so SMM doesn't install
that extra DLL at all: one loader, not two.

To be clear about the split, because the names look alike:

- **ScriptHookV.dll** gives plugins access to the game's functions. Still
  required, still installed - nothing changes there.
- **The ASI *loader*** just finds `.asi` files and loads them. That's the part
  the SMM Loader now does.

It looks for `.asi` files in your **game folder** and in **`scripts\`**, and loads
them in a fixed order every launch. Each one gets a line in `SmmLoader.log`
(`ASI loaded: trainer.asi …`), and if one fails you get the reason there - Diagnose
turns the common ones into plain advice.

**The switch.** The card tells you which `ScriptHookV.dll` your profile actually
loads by its **title**, and one button switches it:

- **SMM Handler** *(default)* - SMM's own handler filling the `ScriptHookV.dll`
  slot (the card is titled "SMM Handler", marked *acting as ScriptHookV.dll*).
  Recommended. Its settings live in **SMH.ini**, editable right on that card.
- **ScriptHookV** - the original third-party ScriptHookV (the card keeps the
  "ScriptHookV" title, flagged *original*). The button reads **Use SHV** to switch
  to it, and **Use SMH** to switch back. Worth trying if a plugin misbehaves under
  the SMM Handler.

Either way the **SMM Loader** is what finds and loads your `.asi` plugins - one
loader, not two. The choice is per profile, and switching reinstalls ScriptHookV so
the right `ScriptHookV.dll` ends up on disk. (If you install ScriptHookV by hand
*outside* SMM, it drops a `dinput8.dll` in the game folder - the SMM Loader notices
and stands down, so your plugins are never loaded twice.)

## Switching one plugin off

Every installed mod that ships an `.asi` has an **ASI on/off** button on its row.
Switching it off keeps the mod installed but tells the loader to skip its plugin
on the next launch - the quickest way to find out which plugin is behind a crash
or a broken feature, without uninstalling anything. Switch it back on and the
plugin loads again.

Big mods often ship **several** plugins, and it's usually just one of them causing
trouble. Expand a mod's row and each plugin it installed has its own small
**on / off** switch in the file list, so you can stop one without giving up the
rest of the mod - its vehicles, peds and everything else keep working. This covers
both `.asi` plugins and the script `.dll`s in your `scripts` folder.

You won't see a switch on every `.dll`. Some are *libraries* - shared code that
another plugin loads by name. Turning one of those off wouldn't disable a feature,
it would just break whatever depends on it, so Simple Mods Manager doesn't offer it.

Your choices are remembered per profile, and they travel with the profile when you
export and import it - so a plugin you switched off because it crashed your game
stays off on the machine you move the profile to.

## When GTA V updates

What a Rockstar game update means depends on which script handler fills your
profile's ScriptHookV slot (the switch above):

**SMM Handler (the default): usually nothing.** SMH finds what it needs inside
the running game each time it starts, so a new game build normally runs right
away. In the rare case an update does need a matching change, that change
ships with the next SMM update - update the app and press **Reinstall** on the
card.

**Original ScriptHookV: wait for its author.** When Rockstar updates GTA V,
ScriptHookV (Alexander Blade's library) stops working until he releases a
matching build. Its log reads `FATAL: Unknown game version`, ScriptHookV never
starts, and so **no script mods load** - including ScriptHookVDotNet, which is
built on top of it. There is **nothing to patch or configure**: ScriptHookV's
version check lives inside its closed binary, so no ini/xml edit can bypass it
safely. SMM helps in two ways:

- **A heads-up at launch.** If your GTA V build is newer than the ScriptHookV
  you have installed, SMM tells you *before* you sit through a broken start -
  the game still launches, but you'll know script mods won't load.
- **A clear Diagnose result.** After the fact, Diagnose turns the cryptic
  `Unknown game version` into a plain explanation and the real fix.

The fix is one of: **wait for the ScriptHookV update** (usually a few days;
SMM pulls the latest from dev-c.com - press **Reinstall** on the card once
it's out), **switch the card to SMH**, or **roll GTA V back** to the previous
build.

A ScriptHookVDotNet **nightly** (below) does *not* help here - it still needs
a working script handler underneath.

## Nightly builds (ScriptHookVDotNet)

ScriptHookVDotNet publishes **nightly** builds - newer than the stable release,
useful when a script mod needs a fix that hasn't shipped yet, or right after a
Rockstar game update when the stable build hasn't caught up.

The **ScriptHookVDotNet** row has a **Nightly** button next to Reinstall. Press
it and SMM installs the newest nightly from the
[scripthookvdotnet-nightly](https://github.com/scripthookvdotnet/scripthookvdotnet-nightly/releases)
releases instead of the stable one.

- The profile **remembers which channel it installed**, and the row shows a
  `nightly` badge so you can tell at a glance.
- Update checks then compare against **that** channel - a nightly is only
  "outdated" when a *newer nightly* exists, never because stable moved.
- To go back, press **Stable** - it fetches and installs the stable build
  (the button reads *Reinstall* only while you're already on stable).

The channel is per profile, so you can keep one profile on stable and try a
nightly in another. Nightlies are untested builds by nature: if a script breaks,
switching back to stable is the first thing to try.
