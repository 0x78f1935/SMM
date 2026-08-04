# What SMM can - and can't - do

Simple Mods Manager is three pieces working together: the **app** prepares your
mods and your game folder, the **SMM Loader** makes the running game actually use
them, and the **SMM Handler** gives script mods the access to the game they need.
All three are original software, built for GTA V Enhanced and shipped inside the
app - nothing is downloaded to get modding working.

This page is the honest summary of what that gets you - and, just as importantly,
what it cannot do. No tool can do everything, and we'd rather you read the limits
here than discover them mid-game.

## What it does for you

- **Installs mods where they belong.** Archives are unpacked and every file is
  mapped to its correct place - loose scripts, `.asi` plugins, whole add-on
  packs, and files that live *inside* the game's archives. Mods written for
  other mod managers (`.oiv` packages) are translated automatically, and even
  many FiveM-packaged mods install as single-player add-ons.
- **Converts Legacy mods to Enhanced.** Most mods are still built for the
  original GTA V. SMM detects that and rebuilds their textures, models,
  drawable dictionaries and vehicle fragments - glass and cloth included -
  into the Enhanced format during installation (collision files work on both
  editions as they are). No other public tool does this.
- **Puts back the game data a Legacy mod would quietly delete.** Mods often
  replace a whole game settings file - the weapon list, the police dispatch
  table, ped loadouts. A mod built for the original GTA V was written against
  an older version of that list, so installing it as-is silently deletes
  everything Rockstar has added since. Anything that still expects one of those
  entries can crash the game. SMM compares each file against your game's own
  copy and puts the missing entries back, while leaving everything the mod
  actually changed alone. It tells you what it restored in the install notes.
- **Fills in vehicle parts a Legacy mod's model never had.** The original GTA V
  let a vehicle get away with a half-finished model; Enhanced looks for the pieces
  by name and crashes when one is absent - which is what a "crashes the moment a
  police vehicle spawns" bug usually is. SMM spots a model like that during
  installation and completes it using your own game's vehicles as the reference,
  leaving everything the model does have exactly as the mod made it.
- **Repairs settings a Legacy mod wrote that Enhanced cannot survive.** Some
  shapes the original GTA V accepted make Enhanced crash outright. A vehicle
  layout with no seats is one: the original game shrugged it off, Enhanced dies
  the instant something spawns a vehicle using it. SMM fills a layout like that
  in from the closest one the same file already defines, and says so in the
  install notes.
- **Runs your script mods on Enhanced.** Script mods talk to the game through a
  library called ScriptHookV. Simple Mods Manager ships its own - the **SMM
  Handler** - installed under the name mods expect, so they can't tell the
  difference. It works out where things live in *your* copy of the game every time
  it starts, which is why a Rockstar update usually doesn't break your scripts.
  Alexander Blade's original ScriptHookV stays a one-click choice per profile if
  you'd rather use it.
- **Loads your `.asi` plugins itself.** The SMM Loader picks them up from the game
  folder and `scripts\` in a fixed order and logs every one - so no second loader
  is needed, and any plugin can be switched off without uninstalling its mod.
- **Resolves conflicts instead of letting mods overwrite each other.** When
  two mods ship the same file you choose a winner, and SMM remembers every
  mod's claim - so uninstalling one mod never rips out a file another mod
  still needs. Some files aren't really a conflict at all: two mods that
  retexture the same dictionary are combined rather than one being thrown away.
- **Backs up before every write.** Any game or mod file SMM installs, replaces
  or patches in the game folder is backed up first, restore points can take you
  back to any earlier state, and the built-in *Default* profile always means
  "the stock, unmodded game". (The loader's own tiny launch-config files -
  `args.txt`, launch options, its transient cache - are the deliberate
  exception: rebuilt every launch, never game content.)
- **Raises the game's limits to match what you install.** Install enough add-on
  content and the game's own configuration would overflow and crash on load -
  SMM scales those limits to what is actually installed, every launch. That
  includes one the game hides: script mods share a small fixed list of "handles"
  for referring to cars, people and objects, and the game ships it far too short
  for a scripted profile - one mod asking what is nearby can want more than the
  whole list holds, and once it runs out, *other* mods start failing quietly
  rather than crashing. SMM sizes that list for you whenever a profile has
  script mods in it.
- **Scans every download.** Files are checked with Microsoft Defender before
  they are unpacked, whether they come from a store tile, a GitHub release, a
  direct link or your Downloads folder. A flagged file is removed and never
  reaches your game; when Defender isn't available the scan is skipped rather
  than blocking your install.
- **Keeps your setup current.** The loader, the handler and the runtimes mods
  depend on are installed and repaired per profile, and update checks cover them
  and every store-installed mod - politely, without hammering the mod sites.
- **Tells you what went wrong.** Every log the game, the loader and the handler
  write lands in one viewer, and **Diagnose** reads them - and the crash dump when
  there is one - to name what faulted and which component was at fault.

## What it cannot do

- **It cannot swap a single texture inside the game's own archives.** A mod
  that ships whole files or packs works fine; a mod that wants to surgically
  replace one texture *inside* an original Enhanced archive does not - that
  content is sealed in a format only the game itself can rebuild. This is an
  Enhanced platform limit, not a missing feature switch.
- **It does not target GTA V Legacy.** SMM installs *into* GTA V Enhanced
  only. Legacy mods are welcome as input (they get converted); a Legacy game
  folder is not a supported target.
- **A few models can't be fully converted.** In rare cases a Legacy model
  uses a feature the converter can't rebuild; that piece is left in Legacy
  form and you get a warning - prefer the mod's Enhanced version when one
  exists.
- **It cannot make a Legacy-only script plugin run on Enhanced.** Asset
  content converts, and the SMM Handler gives plugins the game access they ask
  for - but some compiled plugins (`.asi`) search the game's program code for
  Legacy-specific patterns and stop themselves before they ever get that far.
  Nothing on disk can change what a closed plugin looks for; only its author can
  ship an Enhanced build. When it happens, **Logs → Diagnose** names the plugin
  and explains it; the mod's models and textures still load.
- **It cannot promise you won't be banned online.** SMM is for modding
  **single player**. It reduces accidental online exposure where it can, but
  no tool can guarantee ban immunity - anyone claiming otherwise is selling
  something. Play GTA Online from a vanilla profile.
- **It cannot vouch for what a mod does.** The virus scan catches malware,
  and the conversion makes files loadable - but whether a mod is well-made,
  balanced or compatible with your other hundred mods is up to its author.
- **It cannot stop Rockstar from updating the game.** With the SMM Handler a game
  update is usually harmless - it re-finds its footing in the running game every
  time it starts, and anything that does need a matching change ships with the next
  SMM update. If a profile uses the original ScriptHookV instead, script mods stop
  working until its author releases a matching build; SMM tells you what's wrong
  and pulls the new build the moment it exists.

If something you care about sits on the wrong side of this list, check the
[FAQ](faq.md) - several of these limits have workarounds or "prefer this
instead" advice there.
