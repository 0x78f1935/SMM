# Settings

![The Settings screen: the Game and folders card with both folder pickers and a green encryption-keys banner, above the Preferences list where each setting has its explanation on the left and its switch on the right.](img/SMM_SETTINGS.webp)

Settings is organised into titled cards: **Game & folders**, **Preferences**
(one row per setting, each with its explanation on the left and its switch,
dropdown or button on the right), the three optional **connection**
cards side by side (GitHub, Nexus Mods, Google - each with a small
*how to get one* expander), the **Download cache**, and **About &
credits** at the bottom. (Restore points are not here - each profile lists its
own on its profile page, below the benchmarks.) The background can fade
gently through all of the bundled wallpapers (the default), stay on a single
one you pick, or be turned off for plain dark. The status
bar shows where the app keeps its data: a `SimpleModsManager_data` folder
next to the executable - the whole setup is portable. Set `SMM_DATA_DIR` in a
`.env` file to relocate it (see `.env.example`).

The game-folder line shows your **GTA V Enhanced build** (e.g. `1.0.1158.13`) when
a valid folder is set - worth quoting whenever you report a problem, since a
Rockstar update can change what a mod does.

**Encryption keys.** Just below the game folder, a status line shows whether Simple
Mods Manager has learned your build's encryption keys. Add-on packs and other DLC
content need them, and they can only be read by running the game once with the loader.
When they're missing it reads *"not learned yet"* with a **Launch to set up**
button (reaching the main menu is enough); once learned it shows a green
*"learned ✓"* - the keys are kept in your portable data folder, so they survive
across sessions and you won't be asked again. A **Legacy mod's audio** needs a second
key of its own; Simple Mods Manager works it out from your game the first time it
converts a mod's audio and keeps it in the same folder, so that one never asks you for
anything extra - but it does need the keys above, which is why the same one-time launch
covers both. (The same one-time prompt also
appears as a banner on the dashboard until the keys are learned.)

On that first key-learning launch, if your profile has no add-on packs to prepare,
the game **closes by itself** once it has handed over the keys and shows a small
*"encryption keys obtained - please start GTA V again"* message. Just launch once
more and your mods load from that run. (When the profile does include add-on DLC
packs, SMM instead loads them during that same first launch, so there's no restart.)

**Install debug files.** Off by default. Mods sometimes ship `.pdb` debug symbol
files that do nothing at runtime; Simple Mods Manager skips them unless you turn
this on (useful only if you develop mods).

**Festive seasonal effects.** On by default - the animated seasonal touches,
like snow falling at Christmas, with more through the year. Purely decorative
and never in the way; turn it off here if you'd rather not have the animations.
(On a festive day a short greeting still appears as a banner across the top -
that friendly hello is always there.)

**Profile order.** How your profiles are listed everywhere - the dashboard grid,
the profile picker at the top, the launch menu and the wizard's *Install into*.
Choose *last played first* (the default), *name A → Z* or *name Z → A*. The
reserved **Default** profile is always listed first whichever you pick: it's your
stock, unmodded game and the thing you go back to. A profile you have never
launched from Simple Mods Manager sorts to the end of the "last played" list -
there's nothing to sort it by yet.

**Guided tour.** *Replay the tour* walks you through the app again - profiles,
finding and installing mods, the three install steps, launching, and this page.
On the two install steps it fills the screen with a worked **example** (a conflict
between two mods, and a small install plan) so you can see what they look like
before you have anything staged; it's clearly labelled, it isn't real, and it
disappears when the tour ends. The tour runs once by itself the first time you
start Simple Mods Manager; you can skip it there and come back to it here whenever
you like.

**Mark un-downloadable mods in Browse.** On by default. A few gta5-mods.com pages
keep their actual files on a different website, so Simple Mods Manager's in-app
download has nothing to fetch and stops with a "couldn't find the file" message.
With this on, such a mod is remembered and flagged in Browse - a **Download
location invalid** badge with its **Queue** and **Install** buttons greyed out, on
both the result card and the mod's page - so you don't keep clicking a mod that
can't be installed here (you can still open its page to download it by hand). The
mod is never hidden from your results. Turn this off to treat those mods normally
again; the **Clear marked mods** button below forgets every mod flagged so far.

**Jump straight into Story Mode.** On by default. The mod loader takes the game
straight past the **startup logos and legal notices** *and* the **Enhanced Online
landing screen** (the one where you'd choose between Story Mode and GTA Online), so
every launch drops you into Story Mode with nothing to sit through or click. It
needs the SML loader present (the same one that loads your mods); if a game update
ever moves the code it can't find, it quietly does nothing and the startup plays as
normal. Untick it here to keep the original intro videos and the landing screen.

**GitHub token.** Optional. SMM asks GitHub for version information in several
places - some of the managed dependencies, any mod you installed from a GitHub
release, and SMM's own update notice - and GitHub lets anyone ask only **60 times
an hour per IP address** without signing in, a budget you share with everyone else
on your connection. If it runs out, the check fails with *"rate limit exceeded"*;
your mods and your game are entirely unaffected, and it clears on its own within
the hour.

To stop it happening at all, paste a **personal access token** here:

1. Go to [github.com/settings/tokens](https://github.com/settings/tokens) →
   *Generate new token (classic)*.
2. Tick **no scopes at all** - it only reads public release pages.
3. Copy it into Settings → **GitHub token** → *Save*.

That raises the limit to 5000 an hour. The token is checked with GitHub before it's
saved (so a mis-paste is caught immediately), stored with your other settings in your
portable data folder, and sent to nobody but GitHub. **Clear** removes it and goes back
to anonymous checks.

**Nexus Mods API key.** Optional. Your personal Nexus key lets SMM browse the **Nexus
Mods** tile (and download in-app on Premium accounts). Find it on your
[Nexus Mods API keys page](https://next.nexusmods.com/settings/api-keys) ("Personal API
Key"), paste it into Settings → **Nexus Mods API key** → *Save*, and **Clear** to
disconnect. You can also connect it from Browse → Nexus Mods; either place works. It's
stored in your portable data folder and only ever sent to nexusmods.com.

**Google API key.** Optional, for the **Google Drive** tile. Without one, SMM installs
single Drive files fine but has to scrape the public page for *folders*, which Google
breaks and rate-limits. A free key makes folder downloads reliable: in the
[Google Cloud Console](https://console.cloud.google.com/) make a project, enable the
**Google Drive API**, create an **API key**, then paste it into Settings → **Google API
key** → *Save* (or into the Google Drive prompt). **Clear** removes it. It's stored in
your portable data folder and only ever sent to Google.

**Download cache.** Every mod and dependency you download is kept in a shared
cache, so the same version is fetched from the internet only once and reused
across every profile that installs it - faster installs, less disk, and instant
profile imports for anything you already have. Settings shows how much space the
cache uses and a **Clear cache** button to empty it (SMM re-downloads
anything it needs again later). **Show downloads** expands the full list - every
cached mod and dependency with its version, file name, size and date - and each
row has a ✕ to remove just that download. Removing a cached download never
touches your installed mods; it only means SMM would fetch that file again the
next time a profile needs it.

**Diagnostics (leave off).** One switch turns on everything that helps a bug
report - the loader's file-call trace, graphics probe and crash dump, plus the
handler's exception tracer. It costs performance and makes the logs much
bigger, and the normal logs already contain everything a routine report needs
(versions, your game build, the mods served, which plugins loaded) - so keep it
off unless a maintainer asks, then: turn it on, reproduce the problem, send the
logs.

**SMM updates itself.** The startup version check also asks GitHub for the
newest released SMM (politely, once - same as the mod checks). When a newer
release exists, a small **⬆ v… available** pill (naming the new version) appears next to the version in
the top-left - and a full-width banner across the top of every screen - with an
**Update & restart** button: SMM downloads the new version, verifies it, swaps
itself out and restarts. (The banner's **Later** hides it until the next
launch; the pill stays.) Your profiles, mods and
settings live in the data folder next to the exe and are untouched. (Windows
SmartScreen may warn once about the freshly downloaded exe - expected for a
small unsigned project.) The About card links the releases page if you prefer
updating by hand. No pill means you're current.

At the bottom, **About & credits** shows the app version (with an update link
when a newer release exists), quick links to the documentation and the
bug-report page, and the support row - star it on GitHub, endorse it on Nexus
Mods, like it on gta5-mods, or buy the author a coffee. Most open in your
normal web browser; **👍 Endorse** endorses on Nexus Mods in-app - if your
Nexus account isn't connected yet it asks for your API key first, then
endorses.

Below that, two credit walls give the people behind the project their due:
**The managed dependencies** - one card per dependency Simple Mods Manager
installs and keeps up to date, each with its author, what it does, and a link
to its home: SMM's own Loader and Handler (SMH), plus Script Hook V .NET
Enhanced by Chiheb-Bacha, iFruitAddon2 by Bob74, NativeUI by Guad and LemonUI
by justalemon -
and **Standing on the shoulders of**, the tools whose ideas SMM's own loader
and handler were inspired by (Script Hook V, ScriptHookVDotNet by crosire &
contributors, Simple Mods Loader Enhanced by NativeCoder, OpenIV, CodeWalker,
and of course Rockstar's Grand Theft Auto V). The lists come straight from the
app itself, so a newly added dependency shows up here automatically. This guide
is always available from the **Docs** tab in the top bar.
