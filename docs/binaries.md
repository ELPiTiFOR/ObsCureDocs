# Binaries info

This page documents data structures and tables embedded inside the
game's compiled binaries (executables), as opposed to the standalone
data files documented in the [File Formats](formats.md) page.

## ObsCure

### Font glyph table (Obscure.exe data)

The engine renders all text through pre-rendered bitmap font atlases
in TGA form: `font_holstein16.tga`, `font_holstein18.tga`,
`font_holstein24.tga`, and `font_holstein30.tga`. There are no TTF
or OTF files anywhere in the game. Each glyph in those atlases is
located via a lookup table embedded in `Obscure.exe`.

The original table has 150 entries covering Windows-1252 printable
characters. Two instructions in the engine reference it:

- File offset `0x05AB89` in `Obscure.exe` contains the table's
  virtual address (4-byte immediate).
- File offset `0x05AB90` contains the entry count (4-byte
  immediate).

### GlyphSlot

| Type   | Size | Name      | Description                                                                       |
|--------|------|-----------|-----------------------------------------------------------------------------------|
| uint16 | 2    | Codepoint | Unicode codepoint that this slot represents.                                      |
| uint16 | 2    | Atlas X   | Left edge of the glyph rectangle in the atlas (texture pixel coordinates).        |
| uint16 | 2    | Atlas Y   | Top edge of the glyph rectangle in the atlas (texture pixel coordinates).         |
| uint16 | 2    | Width     | Width of the glyph rectangle (how many texture pixels to read horizontally).      |
| uint16 | 2    | Advance   | Pen advance after drawing this glyph. Combining marks use 0 here.                 |

The four font atlases share a single coordinate space — the same
`Atlas X` / `Atlas Y` values are used by all four font files. In the
versions examined this means all four atlas textures have identical
pixel content.

To add new glyphs (for example, to support a codepoint range outside
Windows-1252) one approach is:

1. Add a new PE section to `Obscure.exe` large enough to hold an
   extended table.
2. Write the extended `GlyphSlot[]` table inside the new section.
3. Overwrite the 4-byte immediates at `0x05AB89` (VA) and `0x05AB90`
   (count) to point at the new table.
4. Pack the new glyphs into the existing atlas TGAs at any free
   region. The atlas is hardcoded to `1024 x 1024` pixels — larger
   atlases are rejected by the renderer.

A separate table reference used by the `font_holstein30.tga`
rendering path lives at a different code offset and must be
redirected independently if the new table also serves that size.
