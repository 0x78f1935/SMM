# Logs & diagnosis

One page for every log the game writes - SMM's own components, the script handlers,
per-mod logs, the Rockstar launcher and crash dumps:

- **Pick a log on the left, read it on the right.** The page is a two-pane
  layout - a compact list of every log file (name, source and size) on the left,
  and the selected log's full, color-coded content on the right - so you're not
  scrolling through everything stacked together.
- **Grouped by source** with the filter chips: **SMM Loader** (`SmmLoader.log`)
  and **SMM Handler** (`SmmHandler.log`, from SMM's own ScriptHookV.dll) are kept
  separate from the **Script handlers** (ScriptHookV, ScriptHookVDotNet), your **Mod
  logs**, the **Launcher** and **Crashes** - so it's obvious which is which.
- **Only the latest run** is shown by default ("Since last launch" chip);
  rotated duplicates (`launcher.01.log`, …) are collapsed behind a toggle,
  and multi-session logs are trimmed to the last session.
- **Crash dumps are limited to your last session.** Windows keeps a pile of `.dmp`
  files from every run (and other apps' crashes) in a shared folder - the list only
  shows dumps from your **most recent game session**, worked out from the newest log
  the game wrote (so it's right even when you launch from Steam or Rockstar rather than
  through SMM), so old, unrelated dumps don't bury the logs.
- **A benchmark that crashes always leaves a dump.** Running any benchmark (Profile →
  Benchmarks) automatically turns crash logging on for that run - you don't need the
  Diagnostics setting enabled - so if the game crashes mid-test, SMM writes
  `SMMCrash.dmp` next to the game and links it right on the benchmark result card,
  next to the vehicle or ped that was on stage when it died.
- Log lines are **color-coded**: errors red, warnings amber, info blue, and
  timestamps, file paths and crash addresses get their own colors.
- **Search in logs…** at the top filters as you type, and **Refresh** re-reads
  everything from disk without leaving the page.
- The log you're reading shows its **full path** with a **⧉ Copy** button, so you
  can paste the exact location into a bug report or open it in Explorer.
- **Diagnose** explains failures in plain language with suggested fixes and
  one-click mod searches. It only reports crashes from your **latest run** -
  crash dumps Windows keeps from days ago (they are global and outlive any
  single session) are not shown as fresh problems once you've launched again.
- **Diagnose reads the crash dump.** When the game crashes hard, SMM writes its own
  crash dump - and Diagnose now opens it to tell you *what* faulted and *where*: the kind
  of crash (a null-pointer access, a fatal abort, running out of video memory) and which
  component was at fault - the game engine, your graphics driver, SMM itself, or a specific
  **mod** (named), so you know exactly what to turn off or update.

Some crashes leave no trace in the game's own logs - the game simply
disappears while loading. Simple Mods Manager also checks the places
**Windows** records crashes (Windows Error Reporting and its crash-dump
folder) and tracks **how long the game actually ran** after you press
Launch. A run that ends seconds after starting turns the profile red, and
Diagnose reports it - naming any installed add-on/DLC packs, the
most common cause of exactly this kind of silent startup crash (often a mod
made for GTA V Legacy that doesn't survive on Enhanced).

## Reporting a bug

The **🐞 Report a bug** button (Settings → About) and the Help tab's
**open an issue on GitHub ↗** link open SMM's GitHub issue form with
everything maintainers need spelled out. One guard first: if
your game hasn't done its one-time key launch yet - or hasn't launched at all
since the keys were extracted - SMM explains that instead of opening GitHub,
because most "my mod doesn't show up" reports fix themselves on that very next
launch. **Report anyway** is always available for problems that clearly aren't
about mods loading, like a crash of SMM itself. When you do report, include
your SMM version and the log (Logs → Diagnose names the likely culprit).

At the end of the Diagnose results there's a **For a bug report** card with a
**Copy report** button. It puts the whole diagnosis on your clipboard as plain
text: every finding, the exact plugin and offset that faulted, which log each one
came from, and your SMM and game build. Paste that into the issue - it saves the
back-and-forth, and it's just as useful in a mod author's own bug tracker when
the problem turns out to be their mod rather than Simple Mods Manager.

Each finding also carries its own buttons. When Simple Mods Manager can tell
which of your installed mods a problem belongs to, you get a link straight to
**that mod's page**. Searches are only offered where they can actually find
something: a mod's name goes to gta5-mods, while an internal name from a log
(something like `TrafficLaws.Class1` - a piece of code inside a mod, not a mod)
goes to a web search, because no mod site has ever listed one.
