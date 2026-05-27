# Constants and identifiers

## ObsCure

### ITEM_TYPE
Item types are encoded as uint32. Every value is left padded with zeros. Here's
the list of all types:
```c
typedef enum
{
    NO_ITEM_ID = 0x0000,
    WEAK_FLASHLIGHT = 0X00C9,
    BAT = 0x00CA,
    OLD_PISTOL = 0x00CB,
    LASER = 0x00CC,
    SHOTGUN = 0x00CE,
    METAL_BAR = 0x00D1,
    LIGHT_GRENADE = 0x00D2,
    GUN_WITH_FLASHLIGHT = 0x00D3,
    EMPTY_SLOT_ = 0x00D4,
    YELLOW_FLASHLIGHT = 0x00D5,
    POWERFUL_FLASHLIGHT = 0x00D6,
    AUTOMATIC_PISTOL = 0x00D7,
    REVOLVER = 0x00D8,
    DOUBLE_BARREL_SHOTGUN = 0x00D9,
    ALUMINIUM_BAT = 0x00DA,
    LASER_GUN = 0x00DC,
    SHOTGUN_AMMO = 0x0130,
    HANDGUN_AMMO = 0x0132,
    STATUETTE = 0x0191,
    SAFE = 0x0192,
    HOOK = 0x0194,
    FUSES = 0x0195,
    WOOD_PLANK = 0x0197,
    NEEDLE = 0x0199,
    SCREWDRIVER = 0x019A,
    PIECE_OF_PAPER = 0x019B,
    LEVER = 0x019C,
    FILM = 0x01F5,
    PHOTO = 0x01F6,
    DOCUMENT = 0x01F7,
    MAP = 0x01F8,
    BASEMENT_PLAN = 0x01F9,
    KEY = 0x0259,
    DISC = 0x025A,
    TAPE = 0x025B,
    PLIERS = 0x025C,
    WHEEL = 0x025E,
    MEDKIT = 0x025F,
    CELLPHONE = 0x0260,
    VIDEO_TAPE = 0x0261,
    REEL_OF_WIRE = 0x0263,
    ENERGY_DRINK = 0x0264,
} item_type;
```

### EXTRA_INFO
Gives more information on documents, photos, maps and statuettes, encoded as
uint32. Note that for maps we have letters, that's because the game uses their
ASCII codes. All values are left padded with zeros.
```c
typedef enum
{
    STATUETTE_PRAYING = 0x01,
    STATUETTE_BROKEN = 0x02,
    STATUETTE_CROSS = 0x03,
    STATUETTE_HANDS_BACK = 0x04,
    PIECE_OF_PAPER_DOC = 0x020601,
    BUILDING_OF_SCHOOL = 0x020800,
    LEONARD_STATE = 0x020801,
    LETTER_FROM_WALT_KERRIDAN = 0x020802,
    THE_MORTIFILIA = 0x020803,
    SUBJECT_22 = 0x020804,
    SHOM_TRANSFORMATION_PHOTOS = 0x020805,
    CENSORED_NEWSPAPER = 0x020806,
    BASEMENT_PHOTOS = 0x020807,
    PRESS_CLIPPINGS = 0x020808,
    FIND_THE_SAFE = 0x020809,
    TYPES_OF_MUTATION = 0x020810,
    LETTER_FROM_WICKSON = 0x020811,
    LETTER_TO_WICKSON = 0x020812,
    BEAUTY_CLUB_ = 0x020813,
    BASKETBALL_PROGRAMMEE_ = 0x020814,
    PHOTO_TWINS = 0x020817,
    EXPEDITION_DOCUMENTS = 0x020819,
    EXPEDITION_PLAQUE = 0x020820,
    NECROLOGY = 0x020821,
    ALAN_DIARY = 0x020834,
    EXPULSION_COMMUNITY = 0x020836,
    ADMINISTRATION_MAP = 'B',
    CLASSROOMS_MAP = 'C',
    LIBRARY_MAP = 'D',
    REFECTORY_MAP = 'E',
    AMPHITHEATRE_MAP = 'F',
    DORMITORY_MAP = 'I'
} extra_info;
```

### DIFF_MODE
The DIFF_MODE is a value encoded as a uint8 (possibly extended to uint32 by
left-padding with zeros) that tells us whether an item is present in specific
difficulties and modes. Modes are "New Game" and "Special Mode". The DIFF_MODE
is an integer where each one of the 4 least valuable bits are flags. Here are
the flags and the weights of their bits:

- 0: Easy difficulty
- 1: Normal difficulty
- 2: Hard difficulty
- 3: Special Mode

If a difficulty flag is set, the item shows up in that difficulty. As for the
Special Mode flag, the only item that has it set to 1 is the laser gun, and its
DIFF_MODE is `8`, so only the Special Mode flag is set, even though the gun
appears in all difficulties (need to do more tests on this).

More examples of DIFF_MODEs:

- `0x7`: `0b111`, the item is shown in all difficulties
- `0x4`: `0b100`, the item only shows up in Hard difficulty
- `0x3`: `0b011`, the item shows up in Easy and Normal

### ITEM_TYPE_SHORT

This is just another way to refer to each ITEM_TYPE, they are encoded as uint32
even though they only take one byte, but they are left padded with zeros. They
are used hide some items, for example.

| ITEM_TYPE_SHORT | Item |
|----|--------------|
| 7A | Shotgun ammo |
| 7B | Handgun ammo |
| 66 | Bad quality flashlight |
| 67 | Yellow flashlight      |
| 68 | Powerful flashlight    |
| CC | Laser                  |
| 69 | Yellow bat             |
| 6A | Metal bar              |
| 6B | Aluminium bat          |
| 6C | Old pistol             |
| 6D | Automatic pistol       |
| 6E | Revolver               |
| 6F | Laser gun              |
| 70 | Laser                  |
| 71 | Shotgun                |
| 72 | Double barrel shotgun  |
| 73 | Light grenade ?        |
| 74 | Gun with flashlight    |
| A2 | Disc          |
| A3 | Sticky tape   |
| A6 | First aid kit |
| AB | Energy drink  |
| 83 | Statuette                       |
| 84 | Safe (unused)                   |
| 85 | Hook (looks weird maybe?)       |
| 86 | Box of fuses and fuse (unused?) |
| 87 | Wood plank                      |
| 88 | Paper cup                       |
| 89 | Compass Needle                  |
| 8A | Screwdriver                     |
| 8B | White image (?)                 |
| 8C | Lever (looks weird maybe?)      |
| 97 | “Filmrolle”                     |
| 9B | Diary? (unused)                 |
| A1 | Nothing (no blue box)           |
| A4 | Pliers                          |
| A5 | Wheel                           |
| A8 | Video tape                      |
| AA | Reel of wire                    |

### ROOM_ID and ROOM_ID_SHORT
These 2 types of constants identify a specific room in the game. ROOM_IDs are
strings that start with a letter and end with 3 digits, while ROOM_ID_SHORTs
are encoded as uint8. Here's a table with all these constants and the names of
the rooms:

| ROOM_ID_SHORT | ROOM_ID | English name and notes               |
|---------------|---------|--------------------------------------|
| 0x01          | a000    | The garden                           |
| 0x02          | a001    | The wood                             |
| 0x03          | a002    | The wood                             |
| 0x04          | a003    | The garden                           |
| 0x05          | a004    | The basement                         |
| 0x11          | b000    | Hall                                 |
| 0x13          | b002    | Meeting room                         |
| 0x15          | b004    | Teacher's room                       |
| 0x16          | b005    | Girl's toilet                        |
| 0x17          | b006    | Boys' toilet                         |
| 0x18          | b007    | Toilets in the teacher's room.       |
| 0x19          | b008    | Cafeteria                            |
| 0x1A          | b009    | Loft                                 |
| 0x1B          | b100    | Loft                                 |
| 0x1C          | b102    | Spanish classroom                    |
| 0x1D          | b103    | The filing room                      |
| 0x1E          | b104    | Boxroom in the filing room           |
| 0x1F          | b106    | Teacher's room (1st floor)           |
| 0x20          | b010    | Boxroom.                             |
| 0x31          | c000    | Corridor (ground floor)              |
| 0x32          | c003    | Toilet                               |
| 0x33          | c004    | Boxroom                              |
| 0x34          | c006    | Physics/Chemistry lab                |
| 0x35          | c009    | Maths/physics classroom              |
| 0x36          | c010    | Administrative office                |
| 0x37          | c011    | Staircase                            |
| 0x38          | c100    | Corridor (1st floor) [Classrooms]    |
| 0x39          | c101    | Administrative office [Classrooms]   |
| 0x3A          | c104    | Living languages classroom           |
| 0x3B          | c105    | Biology classroom.                   |
| 0x3C          | c109    | Office or Mr. Friedman               |
| 0x40          | d001    | Meeting room                         |
| 0x41          | d000    | Corridor (ground floor)              |
| 0x42          | d002    | Friedman's Hideaway                  |
| 0x43          | d003    | Library (ground floor)               |
| 0x44          | d004    | Maths/physics classroom              |
| 0x45          | d005    | Boy's toilet                         |
| 0x46          | d006    | Girl's toilet                        |
| 0x47          | d009    | Living languages classroom           |
| 0x48          | d100    | Corridor (1st floor) [Library]       |
| 0x49          | d101    | Biology room 2 [Library]             |
| 0x4A          | d102    | Abandoned classroom                  |
| 0x4B          | d103    | History classroom                    |
| 0x4C          | d104    | Living languages classroom [Library] |
| 0x4D          | d105    | Staircase                            |
| 0x4E          | d106    | Mezzanine (library)                  |
| 0x4F          | d010    | Secretary's Office                   |
| 0x51          | e000    | Dining hall (ground floor)           |
| 0x52          | e001    | Janitor's room                       |
| 0x53          | e002    | Kitchen (ground floor)               |
| 0x54          | e003    | Video surveillance room              |
| 0x55          | e100    | Dining hall (1st floor)              |
| 0x56          | e101    | Kitchen (1st floor)                  |
| 0x57          | e102    | Teachers' dining hall                |
| 0x58          | e103    | Sick bay                             |
| 0x61          | f000    | Amphitheatre hall                    |
| 0x62          | f001    | Actors' room (ground floor)          |
| 0x63          | f002    | Artists' entrance                    |
| 0x64          | f003    | Amphitheatre                         |
| 0x65          | f101    | Props room (1st floor)               |
| 0x66          | f102    | Projection room                      |
| 0x71          | g000    | Corridor                             |
| 0x72          | g001    | Laboratory 1                         |
| 0x73          | g002    | Guinea pig room 1                    |
| 0x74          | g003    | Guinea pig room 2                    |
| 0x75          | g004    | Prison                               |
| 0x76          | g005    | Laboratory 1                         |
| 0x77          | g006    | Corridor                             |
| 0x78          | g007    | Greenhouse 1                         |
| 0x79          | g008    | Room with the door                   |
| 0x7A          | g009    | Prison                               |
| 0x7B          | g010    | Boiler room                          |
| 0x7C          | g012    | Corridor [F shape]                   |
| 0x7D          | g013    | Corridor [red light and roots]       |
| 0x7E          | g100    | Main corridor [3 homms]              |
| 0x7F          |         | Nursery??? [doesn't work]            |
| 0x80          | g103    | Cell [Lever]                         |
| 0x81          | g104    | Greenhouse 2                         |
| 0x82          | g105    | Machine room                         |
| 0x83          | g106    | Corridor [after Prison]              |
| 0x84          | g107    | Store                                |
| 0x85          | g014    | Experimentation room                 |
| 0x86          | g015    | Cell                                 |
| 0x87          | g016    | Corridor [Basement]                  |
| 0xA1          | i000    | Corridor                             |
| 0xA2          | i001    | Waiting room [Dormitory]             |
| 0xA3          | i002    | Study room [Dormitory]               |
| 0xA4          | i004    | Boys' toilet                         |
| 0xA5          | i007    | Dormitory                            |
| 0xA7          | i100    | Corridor [1st floor Dorm]            |
| 0xA8          | i101    | Dormitory [1st floor]                |
| 0xA9          | i102    | Supervisor's room 1                  |
| 0xAA          | i103    | Supervisor's room 2                  |
| 0xAB          | i107    | Dormitory [1st floor]                |
| 0xB1          | j000    | Sports room                          |
| 0xB2          | j001    | Cloakrooms                           |
| 0xC1          | m000    | Central Courtyard (ground floor)     |
| 0xC2          | m001    | Parking                              |
| 0xC3          | m002    | Exterior                             |
| 0xC4          | m003    | Gym courtyard                        |
| 0xC5          | m100    | Central Courtyard (1st floor)        |

### HVP_VARIANT
Distinguishes the kind of entry inside an HVP TOC. Encoded as a uint32
in MSB. Only one value is known and used:
```c
typedef enum
{
    HVP_VARIANT_FILE = 0x00000001,
} hvp_variant;
```
Directory entries do not use this field at all (they are detected
structurally — see [HVP_Entry](formats.md/#hvp_entry)).

### LNG_ENCODING
Selects between the two text encodings inside an `.lng` entry. Encoded
as a uint8. Always followed by encoding-specific bytes.
```c
typedef enum
{
    LNG_ENCODING_WIN1252 = 0x00,
    LNG_ENCODING_UTF16   = 0x01,
} lng_encoding;
```
When the encoding is `LNG_ENCODING_UTF16`, the very next byte is a
fixed marker (`0x1C`) before the UTF-16 LE codepoints begin.

## Both games

### LString

LStrings are just strings that tell you how long they are. Even though we're
calling it "string", it is NOT null terminated. Here's their format:

| Type    | Size | Name    | Description                                    |
|---------|------|---------|------------------------------------------------|
| uint32  | 4    | Length  | Length of the string in MSB. Let's call it L.  |
| uint8[] | L    | Content | The string.                                    |

