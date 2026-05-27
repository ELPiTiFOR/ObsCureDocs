# Introduction to modding each game

First of all, there are several versions of each game in the franchise, and they
are all different in how they're coded and how their files work. We'll do our
best to specify all the differences so that this documentation can be applied
for any version.

When installing any of the games in the franchise on your computer, you'll see a
bunch of files, but let's focus on the most important ones:

- HVP files
- The executable file

The files with the `.hvp` extension are packages that include a
lot of game files such as dialogues, models, textures, cutscenes, rooms
info, items info, etc. To read and modify these models, textures, etc, you
need a tool to unpack the HVPs, to download such tools check
[this link](https://obscurefans.com/tools.php). If you want a video
tutorial, you can also check
[this link](https://youtube.com/watch?v=iQymvq5d5nc). As a first step, I
recommend unpacking the HVPs and store all the contents in some folder, as a
sort of backup.

A lot of the files inside the HVPs have a non-standard format, created by
Hydravision or Mighty Rocket Studio to meet their needs, so a very important
part of reverse engineering the game is figuring out these formats, so that we
can read the files more easily, and so that we can modify them as we want. To
check all the file formats that have already been reversed, check the
[File Formats](formats.md) sections.

The executable file stores all the functions that are executed during the
execution of the game. It is possible to modify it directly, but it's a hard
task and requires abilities in reverse engineering. It's in x86 (32 bits) machine
code, so in order to read it more easily you should use a disassembler such as
Binary Ninja or Ghidra. The latter has the advantage of having a free
decompiler, which makes everything even simpler. Listing all the functions
and what they do, and documenting every structure used in-game would be a very
interesting thing to add to this documentation.

After explaining the most general concepts, let's dive into each one of the
games.

## ObsCure

Each version of ObsCure has different HVP files. For the Steam version, we have
`cachpack.hvp`, `datapack.hvp`, `kinepack.hvp` and `strmpack.hvp`.

The four HVP archives that ship with ObsCure (the first game) each
hold a different category of data:

| Archive          | Size (approx.) | Contents                                                                |
|------------------|----------------|-------------------------------------------------------------------------|
| `datapack.hvp`   | ~664 MB        | Main game content: text strings, scripts, models, textures, room data.  |
| `cachpack.hvp`   | ~29 MB         | Preload cache textures (HUD, fonts, small icons).                       |
| `kinepack.hvp`   | ~1.1 GB        | Pre-rendered cutscene videos.                                           |
| `strmpack.hvp`   | ~634 MB        | Streaming audio (voice lines, music).                                   |

If you're translating text or replacing UI textures, the file you
care about is almost always `datapack.hvp`. The HUD font atlas
(`font_holstein16.tga`) lives in `cachpack.hvp`.

For the full binary layout of HVP archives see the
[HVP section](formats.md/#hvp-obscure).

Once you have located the HVPs, unpack them with a specialized tool.

Once you have extracted the HVP files, you'll see a bunch of folders with
names such as "_common", "_levels", etc, they all start with an underscore.
It turns out you can make the game read the extracted files instead of the
HVPs, that means that if you modify the extracted files and you make the
game read them, you will play with the modified files.
To do so, you need to move all those folders starting with an underscore
to a new folder that you will name "data" and you will place said folder
in the root directory of the game (so you will have the `data` folder and
the `.exe` of the game in the same directory). You will also need to rename
the HVP files so that the game can't find them. I suggest adding a "2" to
their names so that you don't lose track of which HVP file is which. So
for example `datapack.hvp` will become `datapack2.hvp`.

Now that you have created the `data` folder (and renamed the HVPs), you can
modify the files inside and run the game to check the changes by yourself. For
example, if you modify the texture `data/_common/textures/boy1.bmp` while
keeping the same format (to make sure the game can still read it) you'll see
Stan will have a different appearance.

When it comes to well known formats like `.bmp`, modifying them is not a
challenge. But it's not the case for non-standard file formats like `.it` or
`.tm`, for which you will need a specialized tool like
[Obscure Model Importer and Exporter](https://www.nexusmods.com/obscure2/mods/4)
by Hydra or [ObsCureFileParser](https://github.com/ELPiTiFOR/ObsCureFileParser)
by ELPiTiFOR. To see a list of interesting tools check
[this link](https://obscurefans.com/tools.php).

If there is no tool or documentation for a specific format, don't hesitate to do
some research on it and adding it to this documentation. To figure out the
format of a file, I recommend using one of these approaches, or both:

- Reading the file with HxD or some other software that lets you see the
hexadecimal representation of the bytes, looking for patterns, trying to modify
the file, and running the game to see what changed
- Study the executable file, debugging the game, and trying to find the
functions that read the file you're trying to study, then study those functions
or decompile them to see how exactly the content of the file is interpreted.
