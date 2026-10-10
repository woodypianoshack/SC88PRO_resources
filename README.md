# SC88PRO CHEATSHEET

Offline reference material for the **Roland SoundCanvas SC-88Pro** (1996), built for everyday MIDI sequencing: a single-page HTML cheatsheet plus REAPER bank and drum-map files.

## Repository layout

```
SC88PRO_CHEATSHEET.html          the cheatsheet (open in any browser, works offline)
reaper/
  reabank/
    SC-88Pro_Native.reabank      all 774 Native-map sounds (43 banks, one per variation)
    SC-88Pro_Drums.reabank       the 25 Native drum kits, for the drum track
  drum_note_names/
    PC001_STANDARD_1.txt         one note-name file per drum kit (25 files)
    PC002_STANDARD_2.txt
    ...
```

## The cheatsheet

Open `SC88PRO_CHEATSHEET.html` in any browser. It's one self-contained file (about 150 KB) with no install, build step or server. It has six tabs:

| Tab | Contents |
|---|---|
| **Operation** | Front-panel guide: every function with its button sequence (e.g. `[SELECT] + LEVEL ◀` to copy a part), grouped by task, with manual page references |
| **Capitals** | The 128 General MIDI capital instruments in 16 categories, with how many variations each program has |
| **Instruments** | All 774 Native-map sounds (PC, CC00, name), grouped by category, capitals highlighted |
| **Drums** | All 25 Native drum kits. STANDARD 1 is the reference; every other kit lists only the notes that differ, with the sound each one replaces, plus an optional full map |
| **Insertion FX** | All 64 insertion effects by category, each with a ready-to-send SysEx string and its parameter list, plus step-by-step routing (part assign, sends, real-time control) |
| **MIDI** | Recommended song header, map select (CC32), control changes, RPN, NRPN (including per-drum-note edits), essential SysEx, extra drum channels, reverb/chorus/delay types with ready-to-send strings, a SysEx checksum builder, and channel mode messages |

A search box searches every instrument, drum sound and insertion effect at once (try `808`, `pad` or `rotary`).

## REAPER files

**Program names (`reaper/reabank/`)**

1. Copy the two `.reabank` files somewhere permanent, e.g. REAPER's resource folder (Options → Show REAPER resource path).
2. On a MIDI track going to the SC-88Pro, open the Bank/Program Select dialog and load the bank file for that track:
   - instrument tracks: `SC-88Pro_Native.reabank`
   - the drum track (channel 10): `SC-88Pro_Drums.reabank`
3. Pick a sound by name. REAPER sends CC0 (variation), CC32 = 3 (Native map) and the Program Change.

The two bank files are separate on purpose: drum kits and instruments both use bank 0/3, so one file can't tell them apart. Program numbers in the files are 0-based, as REAPER expects (PC 1 = 0).

**Drum note names (`reaper/drum_note_names/`)**

1. In the MIDI editor: File → Note names → Load note names from file.
2. Choose the file for the kit you're using; files are named by program number and kit, e.g. `PC026_TR_808.txt`.
3. Set the view to named notes to see "Kick 1", "Snare 1" etc. instead of note numbers.

## Quick reference

Selecting a Native-map sound on channel 1:

```
B0 00 vv    CC0  = variation (the CC00 column)
B0 20 03    CC32 = 3 (Native map)
C0 pp       Program Change (PC − 1)
```

Start songs with a GS Reset (`F0 41 10 42 12 40 00 7F 00 41 F7`) or a System Mode Set. Avoid **GM System On** in SC-88Pro songs: it disables Bank Select and NRPN, so variations are silently ignored.

## Conventions

- **PC** numbers are 1–128, as printed in the manual. The Program Change data byte is PC − 1.
- **CC00** is the Bank Select MSB, which the manual calls the variation number.
- **Musical notes** follow Roland's naming, where middle C (note 60) is **C4**. DAWs that call note 60 C3 will show names one octave lower.
- A **`:`** after an instrument name marks a legato-enabled sound.
- Button sequences use the manual's notation: `A & B` press together, `A + B` hold A then press B, `→` next step.
- In the Insertion FX parameter lists, orange parameters follow effect control source 1 and blue ones follow control source 2.
- Drum names marked **(RND)** (STANDARD 3) vary slightly in timbre and timing with each hit.

## Scope and limitations

- The REAPER files have not been tested on every REAPER version. Please open an issue if something doesn't load.

- The instrument and drum lists cover the **Native map** only. The SC-88 and SC-55 compatibility maps are not included.
- Drum kits cover notes 21–99. The extra sounds on notes 0–19 and 97–127 (manual p.170) are not included.
- Insertion effect parameters are listed by name only; value ranges and defaults are in the manual (p.176–186).
- A few instrument names differ from the manual only in spacing (e.g. `Overdrive Gt` vs `OverdriveGt`), due to how the PDF's typesetting extracts.

## Source

All data was extracted from the **Roland SoundCanvas SC-88Pro Owner's Manual**:

- Parameter list and operations, p.150–153
- Instrument list, p.154–161
- Drum set list, p.163–170
- Effect list, p.176–182
- MIDI Implementation, p.188–209

The manual is available from Roland: <https://cdn.roland.com/assets/media/pdf/SC-88PRO_OM.pdf>

## Disclaimer

This is an unofficial, fan-made reference and is not affiliated with or endorsed by Roland Corporation. Roland, SoundCanvas and GS are trademarks of Roland Corporation. Check against the original manual where accuracy is critical. REAPER is a trademark of Cockos Incorporated. Corrections are welcome via issues or pull requests.
