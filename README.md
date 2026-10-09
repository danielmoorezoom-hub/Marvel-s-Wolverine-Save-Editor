[README.txt](https://github.com/user-attachments/files/33254744/README.txt)
Wolverine Save Editor 1.1  (Marvel's Wolverine, build i38, Kyty saves)

Run WolverineSaveEditor.exe (needs .NET Framework 4, which Windows 10/11 already has).
It looks for Kyty's _SaveData\PPSA03671 folders next to itself, one folder up, and in
your Downloads folder, and lists every slot that has a game.save. Or use "Open save file...".

Close the game (or at least go back to the main menu) before saving, so it doesn't overwrite your edit.

Every time you press "Save changes":
  - the old file is copied to  WolverineSaveEditor-backups\<slot>\game.save.<date-time>  next to the exe
    (not inside the save slot, because the emulator rewrites that folder)
  - the change list is appended to changes.log in the same place
  - the new file is read back before it replaces anything
"Restore a backup..." puts any of those back.

Tabs
  Story        "Go to": pick a mission and one of its objectives, press "Go there".
               The story order, every objective and each objective's checkpoint come from the game's own
               mission and objective graphs (35 missions, 700 objectives).
               "Go there" sets the save up the way the game does when a mission is started directly:
               earlier missions complete, this mission in progress at that objective, the objective's
               checkpoint, the mission loadout / lighting / world states the story has set by then,
               and the activity cards to match.
               Also: checkpoint only, character, mission loadout, completion %, New Game+,
               mission and objective states by hand.
  Activities   every activity in the save, with state and map visibility; search and region filter
  Gear         what you're wearing (suit / hat / claws), owned suits & claws, skills, tactics
               (and which are equipped), techniques, XP and points
  Unlocks      Nightmare challenges, collectibles, progression rewards, gallery
  Changes      what will be written, plus the load-safety checks
  Advanced     every field in the save, editable one value at a time
  Game setup   the two lines of commandline.txt that decide whether an edited save loads (below)

"GAME DATA REQUIRED - Game content not fully installed, unable to proceed"
  The game ships in two install chunks. configs/other/lobby.config names the last objective of the
  first chunk: GP_TEL_COMPOUND_ARKADY_CINE_OUTRO, the end of the Telambang prologue. Once a save is
  past it, the game asks the console (PlayGo) whether the rest is installed before it lets you
  continue. The emulator does not answer, so the game refuses.
  The line  { Engine.PlayGoEnable=false }  in commandline.txt switches that question off.
  The editor checks for it whenever a save is past the prologue, offers to add it when you save,
  and shows the state on the Game setup tab. It keeps a copy of commandline.txt before each change
  and never overwrites commandline.txt.original.

  A  -checkpoint NAME  line in commandline.txt makes the game start at that checkpoint whatever the
  save says. The editor warns when it disagrees with the save and can remove it.

Load-safety checks (run before every save)
  - the checkpoint exists in the game's level data (an unknown name drops you outside the Princess Bar)
  - the checkpoint belongs to the mission that is in progress
  - the install check is off when the save is past the prologue
  - commandline.txt does not force a different checkpoint
  - campaign is kBaseGame and the mission loadout is one of the game's 35

Not everything could be tried in the game itself. What was checked: files are written byte for byte
when nothing is changed; a "Go there" to the post-game from an untouched new-game save comes out the
same as the game's own post-game save apart from internal id numbers; all 700 objective jumps write
and read back cleanly.

Only one playable character exists in this build ("Wolverine", in configs/hero/hero_CharacterListConfig).

source\ holds the C# code, the data taken from the game files (gamedata\) and the script that turns
that data into StoryData.cs.
