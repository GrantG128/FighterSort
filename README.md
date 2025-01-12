# FighterSort

FighterSort aims to be a quick and easy way to batch organize character mods for Super Smash Bros. Ultimate. It is intended for users who have a very large number of mods (hundreds).

## How Does It Work?

Using a combination of CSharpM7's [ReslotterGUI](https://github.com/CSharpM7/reslotter), [a spreadsheet that keeps track of your mod info](https://docs.google.com/spreadsheets/d/1aaScKKdMVOpkFsszZ-uI_kb3F-kEVy_W5SYR6iWR1I0), and your own [ArcExplorer](https://github.com/ScanMountGoat/ArcExplorer) export, FighterSort organizes all of your mods for a character, all at once, how you want them to be organized, ready to drag and drop onto your SD card. The output is in a new folder, so the original mods are unchanged.

There's some initial setup, but it's well worth it if you have a lot of mods since everything after this initial setup is automated for you. For more info, see the tutorial below.

### What It Can Do

- Change the slots of all your mods
- Export specific slots from a multi-slot mod (e.g. if a mod has 8 slots but you only want 3 of them)
- Handle extra slots above c07
  - This is currently bugged; sometimes it works fine, sometimes it doesn't. You can use the `-v` flag when running FighterSort to only use vanilla slots and ignore extra, or `-e` to force everything into extra slots.
- Fix normally-incompatible swaps (e.g. putting a female Inkling mod on a male Inkling slot) by copying the necessary model files from an ArcExplorer export
- (Re)generate config.json for each mod
- Issue a warning when a vanilla slot mod conflicts with an extra slot mod
- Automatically convert all-slot effects into one-slot
- Generate msg_name.xmsbt to add a custom name and/or Boxing Ring title for each mod
- Edit ui_chara_db.prcxml to change each character's number of slots

### What It Can't Do

- Add new CSS character slots (it does not add character slots to the CSS, but it does copy+rename new character mods as needed (this is untested, but I think it should work))
- Add single-slot victory themes
- Add single-slot announcer calls (vc_narration_characall_SLOTNAME is added to ui_chara_db.prcxml, but you'll have to add the actual announcer call yourself; if you don't, that slot's announcer call with be silent)

### What It Will Never Do
- Mii Fighters
- Anything that doesn't involve characters (e.g. stages)

## Tutorial

### Part 1: First Time Setup
If you haven't done so already, use ArcExplorer to extract a `data.arc` from Smash Ultimate. This is used to copy missing model files when needed for certain slot swaps. You only need to keep the following directories:
```
/export
    /fighter
    /sound
        /bank
        /param
```
Also, each fighter directory contains four folders: `ai`, `model`, `motion`, and `param`. You only need `model`.
The sound directory is currently not needed, but may be used in a later update.

### Part 2: Once Per Character
First, organize your mods for each character. The directory names are important, as that is how the sorter will know what the mods are. Follow this structure:
```
/Smash Mods (this can be named anything)
    /[Character] CHARACTER_NAME
        /[CHARACTER_NAME] MOD_NAME [THE ORIGINAL SLOT(S) AS IT WAS RELEASED]
            /fighter
            /ui
            ...
        ...
        key.tsv
    ...
```
Here's an example for Captain Falcon:
```
/Smash Mods
    /[Character] Captain Falcon
        /[Captain Falcon] Afro Falcon [c05]
            /fighter
            /ui
            ...
        /[Captain Falcon] Blaziken [c00-c07]
            /fighter
            /ui
            ...
        /[Captain Falcon] Retro Captain Falcon [c01,c03,c05,c07]
            /fighter
            /ui
            ...
        key.tsv
```
I recommend keeping your unsorted mods on your PC, then copying the sorted output to your SD card.

Next, you may have noticed `key.tsv`. To create this, open the provided [Google Sheets document](https://docs.google.com/spreadsheets/d/1aaScKKdMVOpkFsszZ-uI_kb3F-kEVy_W5SYR6iWR1I0), make a copy for yourself if you haven't done so already via `File -> Make a copy`, and go to the sheet that matches your character. Fill in the info for your mod (column F is determined by the other columns' values, while G and H are optional). Instead of typing this info manually, you can also download [info_getter.py](https://github.com/Mode8fx/FighterSort/blob/main/oneslotnamer.py), put it in your character's directory alongside that character's mod folders, and run it; this will generate a text file that you can copy and paste into the spreadsheet.

Next, fill in Column E for each mod, indicating which slot it should replace/add. Set the value to X if you want to skip this mod. Column J should be changed to either "Echo Slot" or "New Character" if either of those is true.

Finally, go to `File -> Download -> Tab Separated Values (.tsv)`. Save this file as `key.tsv` in the same directory as your mods for the character in question, as shown above.

### Part 3: Running the Sorter
Open `FighterSort.exe`. If this is your first time running it, you will be asked to point to your ArcExplorer `export` folder, along with an output directory where you sorted mods will go (I recommend something like `[All Characters] Output`).

You will be asked to open your character mod directory (e.g. `C:/Smash Mods/[Character] Captain Falcon`). Do that, wait a few seconds, and copy the new contents of your previously-defined output directory to your SD card's Ultimate directory at `sdmc://ultimate`. **And that's it!**

One more thing: You may notice that the output directory now includes a newly-created `ui_chara_db` folder. This is a mod that only contains a single `ui_chara_db.prcxml` file plus its parent directories. This file keeps track of how many slots each character has, along with which slots should use which announcer voice clips (you'll have to add those yourself if you want them). This file will be automatically modified every time you run the sorter, so instead of needing to change it yourself, you simply have to copy it to your SD card alongside your other mods whenever you use the sorter.

## Credits
The following resources were originally made by other people:

#### reslotter.py
- Original code by [BluJay](https://github.com/blu-dev) and [Jozz](https://github.com/jozz024/ssbu-skin-reslotter)
- Modified by [Coolsonickirby](https://github.com/Coolsonickirby) to add support for dir addition for ReslotterGUI
- Further modified by Mode8fx for automation/integration with FighterSort

#### reslotterGUI.py
- Original code by [CSharpM7](https://github.com/CSharpM7/reslotter)
- Modified by Mode8fx for automation/integration with FighterSort

#### dir_info_with_files_trimmed.json
- Original file by [CSharpM7](https://github.com/CSharpM7/reslotter)

#### Hashes.txt
- Original file by [Smash Ultimate Research Group](https://github.com/ultimate-research/archive-hashes)
