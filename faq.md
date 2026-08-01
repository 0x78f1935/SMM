# FAQ

These are the same answers the ❓ **Help** tab shows inside the app, as quick
expandable cards.

## Getting started

**Where is my data stored - and how do I move SMM?**
Everything lives in the SimpleModsManager_data folder next to the executable:
profiles, backups, restore points, settings, logs. The whole setup is
portable - move the exe together with that folder and nothing is lost.

**How do I share a profile with a friend?**
Open the profile and press **Export**. That writes a single `.smmprofile` file
- a **recipe** of your profile: which mods it uses and where they came from,
plus your tweaked configs and conflict choices. It never contains the mod files
themselves (SMM doesn't redistribute other people's work); importing
re-downloads each mod from its source, so the file stays small enough to send
anywhere. It is encrypted, so only Simple Mods Manager can read it and any
tampering is caught on import. Your friend takes it with **Import profile…** on
the Dashboard, next to *Scan downloads* - that always creates a **new** profile
and never touches their game folder until they switch to it.

## Installing mods

**My mod is not installed correctly.**
Simple Mods Manager follows the layout of the mod's archive plus any
installation instructions it can find - the storefront page's description,
readmes and instruction files inside the download. When a mod still lands in
the wrong place, the archive usually ships no usable instructions at all: ask
the mod author to include (or update) installation instructions on their
storefront page or in a readme inside the archive. You can also place the
files manually - and report the mod so the automatic mapping can be improved.

**Windows Defender flagged my download.**
Simple Mods Manager scans every download with Microsoft Defender BEFORE
unpacking it - a detected threat is removed and the install aborts, so a
flagged file never reaches your game. If Defender quarantines something,
don't restore it: get the mod from its official page instead.

**Windows flagged SMM's own loader (hid.dll).**
This is a false positive on Simple Mods Manager's own loader, not a real
infection. Defender's machine-learning heuristic (a name ending in `!ml`, such
as `Bearfoos`) flags the loader because it does the same things an injector
does - it is an unsigned proxy DLL that hooks the game's file reads and loads
`.asi` plugins into the game - and because a freshly built copy has no track
record yet. It is the loader SMM builds and ships itself; nothing was
downloaded. To use it, restore it from **Protection history → Allow** and add
a Defender **exclusion** for your GTA V folder so it is not quarantined again.
This is separate from the download scan above, which still protects every mod
you install.

**Can I use Legacy (gen8) mods?**
Simple Mods Manager manages GTA V Enhanced. When a mod ships both editions,
the Enhanced files are installed and the Legacy ones skipped automatically.
When you install a Legacy add-on pack, SMM detects it, makes the archive load
on Enhanced, and rebuilds its Legacy textures, models, drawable dictionaries
and vehicle fragments (`.ytd`/`.ydr`/`.ydd`/`.yft` - including glass and
cloth) into the Enhanced format automatically; collision files (`.ybn`) and
nav meshes (`.ynv`) work on both editions as they are. In the rare case a
model uses a feature the converter can't rebuild yet, SMM leaves that piece in
Legacy form and warns you - prefer the mod's Enhanced version if one is
available. One thing no tool can convert: a compiled script plugin (`.asi`)
built only for Legacy may refuse to start on Enhanced - **Logs → Diagnose**
names it when that happens, and only the mod's author can ship an Enhanced
build.

**How are mods made compatible - on my PC, or does SMM ship patched copies?**
Everything happens on your own PC. SMM downloads a mod from its original page
and, while installing it, converts whatever needs converting - right on your
machine, for your copy of the game only. SMM does not include, re-upload or
redistribute anyone's mod: no mod files ship inside SMM, and nothing you
install is shared anywhere. The result is the same as if you had edited your
own copy by hand following a tutorial, just automated - no creator's work is
ever republished, so no redistribution permission is needed. Sharing a
profile follows the same rule: the export is only a list of which mods to
fetch from where, never the files themselves.

**Can I download mods directly from mod sites?**
Yes - the Browse tab is a built-in store view. You can browse and search
gta5-mods.com directly, browse Nexus Mods with your own free API key, and
install straight from a GitHub release, a Google Drive share link, or any
direct download link you paste. Every download goes through the same
pipeline: scanned with Microsoft Defender before unpacking, then straight into
the install steps, where you review everything before a single file is written.
If you would rather collect several mods first, **Add to queue** puts them in
the 📥 **Queue** and installs them together in one pass.

**Can I keep using the app while it downloads and installs?**
Yes. Queued downloads run in the background - several at once - and the 📥
button in the top bar counts what is running; click it to open the Activity
drawer, where every background task shows its progress and a still-running
download can be cancelled. Starting an install from the wizard works the
same way: the wizard closes, the install runs in the background while you
keep browsing or reading logs, and a summary appears when it finishes. The
queue itself survives a restart - mods you queued but never installed are
restored on the next start and a message says so. Only the operations that
rework the whole game folder (switching profiles, restoring a restore
point, importing a profile) still hold a progress window, because nothing
may interfere with them halfway. Closing the window while an install is
writing is safe too: SMM finishes that write first, tells you it is doing
so, and then closes itself - a plain download is simply cancelled.

**Does SMM pick the right files for Enhanced automatically?**
SMM manages the Enhanced edition and sorts editions at the file level, where
it matters. Whichever archive you download, SMM looks inside: when a mod
ships both Legacy and Enhanced content, the Enhanced files are installed and
the Legacy ones skipped; when it ships only Legacy content, SMM converts what
it can automatically (see the Legacy question above). When a mod page offers
several downloads, SMM **pre-selects the newest Enhanced one** for you and warns
you if the mod is Legacy-only; a version picker on the mod's page lets you take
a different file if you want to.

## Loaders & dependencies

**Does SMM use ScriptHookV?**
ScriptHookV (by Alexander Blade) is the community library most script mods
use to talk to the game. SMM fully supports it - and by default fills that
role with its own component instead: the SMM Handler (SMH), an original
replacement built specifically for GTA V Enhanced and shipped inside SMM.
Mods can't tell the difference: the installed file keeps the name
`ScriptHookV.dll`, because mods look for exactly that name. If you prefer the
original, one click on the ScriptHookV card switches a profile to Alexander
Blade's ScriptHookV and SMM downloads it from its official page (dev-c.com).

**Does SMM replace ScriptHookV and third-party mods-folder plugins?**
Yes - that is what SMM's two built-in components do. The SMM Loader takes the
job that third-party rpf-loading plugins perform on the Legacy edition:
making the game read modified files from the mods folder.
Those plugins were built for Legacy GTA V; on Enhanced the SMM Loader
provides that ability itself, so none of them are needed or installed. The
SMM Handler covers the ScriptHookV role (see the previous question). Both are
original SMM software - and the original ScriptHookV stays a supported choice
per profile.

**What loads my .asi plugins - and what is the switch on the ScriptHookV card?**
The SMM Loader loads them: it picks up every .asi plugin in the game folder
and in scripts\, in a fixed order, and writes each one to its log
(SmmLoader.log - 'ASI loaded: …'). That is why SMM doesn't install the extra
loader DLL ScriptHookV bundles (dinput8.dll/xinput1_4.dll) - two programs
must never fight over the same job. The switch on the ScriptHookV card
chooses something different: which `ScriptHookV.dll` the profile uses - SMM's
own handler (SMH, the default) or the original ScriptHookV. You can also
switch a single plugin off from its row in the mods table - the mod stays
installed, the loader just skips it, which is the quick way to find out which
plugin is misbehaving.

## In the game

**Why do some profiles show Sync buttons next to the vehicles - and mine doesn't?**
Those buttons appear only when the mod they feed is installed in that profile.
**📝 Sync spawn list** appears when the
[All MP Vehicles in SP](https://www.gta5-mods.com/scripts/all-mp-vehicles-in-sp)
script is installed (its `scripts\NewVehiclesList.txt`) - one click adds every
installed add-on vehicle to its spawn list, so they can appear out in the
world. **🧾 Sync Native Menu** appears when
[NativeCoder's Native Mod Menu](https://www.gta5-mods.com/scripts/native-mod-menu-asi-enhanced-nativecoder)
is installed (its `menu.ini`) - the same idea, filling the menu's
`Addon_Vehicles` list. **🎛 Sync Menyoo** appears when
[Menyoo](https://www.gta5-mods.com/scripts/menyoo-2-0) is installed - it fills
Menyoo's added-vehicle list, so your add-ons show up under *Vehicle Spawner*
(Menyoo itself you can install with one click from the profile's **Actions**
card). Install the mod and the matching button shows up; without it there is
nothing to fill, so the button stays hidden.
All three take a backup first and keep the list in step both ways - new vehicles
are added, and entries for synced vehicles you have since removed are cleaned
up again. The mod's own stock list, your hand edits and vehicles you added in
Menyoo yourself stay untouched. Safe to press any time.

**Why does loading take longer with add-on packs?**
The game keeps a cache describing its archives, and it only reads that cache -
so a pack SMM added would never be noticed. While your profile has add-on packs
installed, SMM therefore switches that cache off, and the game scans its
archives fresh instead. That scan is what costs the extra seconds, and it
happens on every launch, not just the first - it is the price of the packs
being seen at all. Your original cache is backed up and put straight back the
moment the profile has no add-on packs left, so a script-only or vanilla
profile loads at full speed again.

**Some of my mods don't show up in-game - do I need to launch twice?**
Normally no - mods apply on the **first** launch. The one nuance is a
brand-new setup: the game has to run once with the loader so it can extract the
game's encryption keys (the dashboard banner guides that one-time launch -
reaching the main menu is enough). A Legacy mod's audio needs those same keys
before it can be converted, so a mod whose sound is missing on a brand-new setup is
usually this. What that launch does depends on your profile:
with add-on DLC packs the loader holds the loading screen while SMM finishes
preparing, so they load right away; with none, the game closes itself the moment
it has the keys and shows a *"please start GTA V again"* message - just launch
once more and everything loads.
After that bootstrap it's first-launch from then on: SMM prepares the game's
launch options before it starts (it switches GTA to Win32 file reads so the
loader can serve your modded archives). If a mod is still missing right after
installing or switching profiles, close the game fully and launch once more - a
Rockstar/Steam update can reset that launch option, and SMM re-applies it
automatically on the next start.

## When things go wrong

**The game crashes on startup after installing mods.**
Open the profile and check the hard-dependencies panel: an **amber** row means
files are missing, and any row offering **Update** or **Repair** is one click
from fixed. (Amber can also read *"can't check yet"*, and red *"check failed"* -
both only mean SMM couldn't ask GitHub for the latest version, not that
anything is broken. See the rate-limit question.) Then
check **Logs → Diagnose**: it names the most likely culprit with the evidence
line. Still stuck? Use **Launch vanilla (no mods)** from the launch menu to
confirm the game itself is fine, then switch back to your profile and remove
the most recently installed mod first.

**The game freezes during play and then closes itself.**
A freeze is not a crash, and the difference matters: the game stops responding,
waits about a minute, then shuts itself down. That is almost always a script mod
making a call that never comes back - speech, network or a menu library are the
usual ones - because scripts take turns on the game's main thread and one that
never finishes its turn stops everything. Open **Logs → Diagnose**: SMM reads the
crash dump and names the script, or the part of Windows it hung in, and it will
tell you when a mod is **not** to blame too. If it points at a script mod, turn
that one off (the ASI switch, or **Remove**) and play again. It is not the last
mod you saw loading - that one had already finished.

**The game shows a black screen and never loads.**
That is almost always the game running out of one of its internal limits while it
builds the world - the classic fix people apply by hand is an "increased limits"
gameconfig.
Simple Mods Manager does this for you: it measures how much your profile actually
adds and raises the game's pools, heaps and streaming budget to match, every
launch, without you installing anything. You can still install a gameconfig mod if
you prefer; SMM keeps its values and only ever raises them further when resources
are available, never lowers those values.

**The game updated and my mods stopped working.**
That depends on which handler your profile uses (see the ScriptHookV
question). With the default SMM Handler, a Rockstar update is usually no
problem: the handler adapts to the running game each time it starts, and if
an update ever does need a matching change, it ships with the next SMM
update. With the original ScriptHookV selected, the classic rule applies:
after a game update ScriptHookV stops working until Alexander Blade releases
a matching build (its log says 'Unknown game version' and no script mods
load) - wait for that release and press Reinstall on the ScriptHookV card,
switch the card to SMH, or roll the game back. A ScriptHookVDotNet update
alone won't help - it still needs a working handler underneath. Your mods and
configs stay safe in the profile either way.

**A dependency says "can't check yet" / GitHub is rate-limiting me**
Several of SMM's version checks go through GitHub - some of the managed
dependencies, mods installed from GitHub releases, and SMM's own update notice -
and GitHub allows only 60 anonymous requests an hour per IP address, shared with
everyone else on your connection, so it can run out through no fault of yours.
Nothing is wrong with your mods, your game or that dependency: we simply weren't allowed to ask yet. The
card tells you roughly when the limit lifts (it resets every hour) and everything keeps
working meanwhile. SMM only asks once at startup and remembers the answers, so this
should be rare. To remove the limit for good, press "Fix this permanently…" on the card,
or go to Settings → GitHub token: it takes a minute, needs no permissions ticked, and
raises the ceiling to 5000 checks an hour.

## About the project

**What is the goal of these replacements - will they grow beyond the originals?**
The first goal is reliability: your mods should keep working without waiting
on third-party updates - the SMM Handler finds what it needs inside the
running game when it starts, so a new game build usually works right away,
and any change that is needed ships with SMM itself. Beyond that, the
replacements already do things the originals don't, wherever it helps you
manage mods: an in-game loading screen that shows your profile's mods and
credits their creators, a per-plugin on/off switch, crash reports that name
what a misbehaving mod was doing, and per-vehicle toggles for add-on packs.
That is the pattern going forward - functionality grows where it makes
modding easier or safer, and everything shipped is listed in the changelog.

**Does SMM use CodeWalker?**
Not the program itself - CodeWalker is never run, bundled or required. But
SMM's understanding of the game's file formats builds on research the
CodeWalker project (by dexyfex and contributors) published as open source:
some conversion methods and data tables in SMM are adapted from CodeWalker's
MIT-licensed code. That is why CodeWalker is listed in SMM's credits.

**What is SMM written in - and do I need to install anything for it to run?**
The engine - everything that scans, converts and installs mods - is written
in Python and compiled into a single self-contained Windows program. The two
components that run inside the game (the SMM Loader and the SMM Handler) are
written in C++, the language of the game itself. The interface is built with
web technology (HTML/JavaScript) shown in a desktop window. You install
nothing extra - no Python, no frameworks: the released
`SimpleModsManager.exe` carries everything it needs. The only requirement is
Windows' WebView2 runtime, which ships with Windows 11.

**Is the source code open source and available?**
Not yet - but there is a written path to it. The full source (the SMM engine,
the SMM Loader and the SMM Handler) converts to the **GPLv3** open-source
license once community support through
[Buy Me a Coffee](https://buymeacoffee.com/undefined418) reaches **€75,000** -
the community collectively buying the source code. That is simply the price it is
for sale at. What it buys, counted rather than estimated: about 49,000 lines of
Python engine across 98 modules, about 26,000 lines of C++ across the
original loader and the original script handler, about 10,500 lines of UI, 2,475
automated tests, and capabilities no other public tool ships (Legacy→Enhanced
conversion of models, textures and vehicle fragments, OIV→SML translation, an
original loader and an original ScriptHookV-compatible script handler). GPLv3 is
deliberate: once the community has bought the code, copyleft keeps it open -
nobody can close a fork and resell the work. Until then the source stays all
rights reserved; the pledge itself is written down in the LICENSE - readable
right here under [License](license.md).
