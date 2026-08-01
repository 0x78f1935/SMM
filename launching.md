# Launching the game

## First-time setup: learn your game's keys

The very first time, a banner asks you to **Launch to set up**. Some mods (add-on
packs and other DLC content) need Simple Mods Manager to know your specific game build's
encryption keys, and those only become available after the game has run once with
the loader. Click the button and the game starts; what happens next depends on
your profile - either way you only ever do this once, and browsing and
downloading mods work without it:

- **Profile has add-on packs:** the loader briefly holds the loading screen
  while SMM finishes preparing, and the packs load on that same launch -
  reaching the main menu completes the setup.
- **No add-on packs (a fresh setup, the usual case):** the moment the keys are
  learned the game closes itself and shows a *"Please start GTA V again -
  your mods will load from now on"* message. That is expected - just launch
  once more and everything loads from then on.

Skipped the banner and just pressed **Launch**? That works too - the same
one-time setup simply happens on that launch, with the same two outcomes; SMM
tells you what to expect right there. You'll never see it again afterwards.

The header's **🚀 Launch** button starts the game with the active profile
(via PlayGTAV.exe). From the moment you click it, a fullscreen progress modal
tracks the whole start - preparing the profile, waiting through the Rockstar
launcher (which can take a while on sign-in or launcher updates), then *GTA V
is loading* - and it stays up until the actual game window appears. The **▾**
dropdown can:

- *switch to another profile and launch it* in one go,
- **Launch vanilla (no mods)** - switches to the built-in *Default* profile
  (an empty mod surface: the stock game) and launches. Switch back to your
  mod profile from the same dropdown afterwards.

Launching a profile that has mods first shows a short **modding-at-your-own-risk**
reminder - click **Understood** to start it, or **Cancel** to back out.
Launching vanilla skips it.

While the game runs, the Launch button turns into ⏹ **Stop** - one click
closes the game (and the Rockstar launcher if it's still open), just like
Steam's Stop button. If Stop reports that it couldn't close the game, the
game is running with higher privileges than Simple Mods Manager - start SMM
as administrator (right-click → *Run as administrator*), or close the game
from its own window.

Anything that changes the **active** game folder - switching profiles, installing
or uninstalling a mod, updating a dependency, or restoring a restore point - is
refused while the game is running, with a **"Close GTA V first"** popup, so no
game file is replaced mid-session (that can corrupt your install). Installing into
a *different* profile you're not currently playing still works.

Launching through the manager is what feeds the health dots: after the
session ends the dashboard re-evaluates the logs and updates the color.

## The loading-screen dashboard

When you launch a profile that uses **SMM Handler** (the script-mod runtime), the
loading screen shows a little dashboard over the artwork while the game streams in:
the SMM logo, what's new across the last few releases, a thank-you, and a **Mods
loaded** panel listing every mod in the profile you launched and its author. Its
panels are slightly see-through, so the loading wallpaper still shows behind them.
Huge modpacks are fine - it lists as many as fit, then "…and N more". It appears
**only on the loading screen** and disappears the moment you're in the game. It's a
built-in part of SMM Handler - the app has no switch for it.
Profiles without SMM Handler don't show it. (It appears on the loading screens after
the game's initial start-up load, not during that very first one.)

To keep the loading screen and your mods stable, SMM turns **frame generation** off
in GTA's graphics settings while you play a **modded** profile - frame generation and
the loading-screen overlay both hook the game's display in a way that clashes.
**DLSS/FSR upscaling and every other graphics setting are left exactly as you set
them**, and the vanilla **Default** profile isn't touched at all. If you want frame
generation back for unmodded play, launch the **Default** profile.

Every launch also clears the loader's temporary `.tmp` cache in the
game folder automatically - a leftover cache from a previous mod setup can
crash the game, and the loader rebuilds it fresh on startup anyway.

## Add-on packs

Some mods add brand-new content - vehicles, maps, weapon packs - as an
"add-on pack". Install one into a profile and Simple Mods Manager registers
it for you; you never edit game files by hand.

Two things to expect from a profile that has add-on packs:

- **loading takes a little longer, on every launch.** The game keeps a cache
  describing its archives and only reads that cache, so a pack Simple Mods
  Manager added would never be noticed. While your profile has add-on packs, SMM
  switches that cache off and the game scans its archives fresh instead - that
  scan is the extra few seconds, and it is the price of the packs being seen at
  all. Your original cache is backed up and put straight back the moment the
  profile has no add-on packs left, so a script-only or vanilla profile loads at
  full speed again.
- the pack is normally ready on **that same launch** - no second launch needed
  (on a brand-new setup the odd straggler can take one more).

If a pack was built with tooling that GTA V Enhanced can't read as-is, Simple
Mods Manager prepares it automatically (keeping a backup) so it loads without
crashing. If a pack simply can't be made compatible, you're told up front
instead of the game crashing.

If you launch again the instant a previous session closes, Windows may still be
releasing the game's files and Simple Mods Manager can't finish preparing a pack.
Rather than let the game crash (an `ERR_FILE_PACK` error), it asks you to **wait
a few seconds and launch again** - the second try succeeds once the files are
free.

## "Some files could not be written into the game's archives"

Most mods don't ship loose files; they ship *replacements for files inside the
game's own archives*, and Simple Mods Manager writes them in for you. If it ever
can't place one, it says so at launch and **names the file** rather than letting
you find out from a crash - because everything else (the mod list, the install
report) would still show the mod as installed.

Treat that message as a bug worth reporting, not something to work around. A mod
that can't fully install is a gap Simple Mods Manager should close, and the file
names in the message are exactly what's needed to close it. The rest of the
profile still loads normally in the meantime.
