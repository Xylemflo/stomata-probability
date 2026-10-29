# Stomata Note Probability for REAPER

Stomata gives selected MIDI notes an independent probability of playing. The JSFX is the only interface you use. Its companion manager runs silently in the background as a REAPER startup action and supplies the MIDI Editor access that a JSFX does not have.

## Install

1. Copy `Effects/Stomata Note Probability.jsfx` into REAPER's Effects folder, for example `Effects/Stomata Note Probability/` under REAPER's resource path.
2. Load `Scripts/Stomata Note Probability Manager.lua` through **Actions → Show action list → ReaScript: Load…**.
3. Set that action as REAPER's global startup action. With SWS installed, use **Extensions → Startup actions → Set global startup action** and paste the script's command ID. It starts a windowless service; it does not open a separate manager window.
4. Add **JS: Stomata Note Probability** to the MIDI track before the instrument.

## Use

1. Open the MIDI item in REAPER's MIDI Editor and select one or more notes from the same take.
2. Open the Stomata JSFX window. It shows a green **Manager connected** indicator when the startup action is running.
3. Click **Refresh selection**. The interface loads the selected notes and their existing probabilities.
4. Drag any probability bar, or use **Set 0%** or **Set 100%** for the selection.
5. Click **Apply**. The values are saved with the project and used during playback.

`100.0%` always plays and `0.0%` never plays. Other values are randomized whenever the note is encountered.

Each JSFX instance keeps its own loaded selection. To edit a different track or item, open that instance, select the target notes in the MIDI Editor, and click **Refresh selection** in that instance. Its display and pending edits remain separate from every other Stomata window.

## Storage and editing behavior

The manager saves each non-100% assignment as a MIDI text event immediately before the note, such as `P 33.3% C1 N60`. The extra channel and pitch information makes separately selected notes in a chord reliable and keeps the metadata out of the instrument input.

If the selected notes are changed, moved, copied, or deleted after a refresh, **Apply** is deliberately rejected. Click **Refresh selection** to load the current notes before applying. Exact duplicate MIDI notes with the same start time, channel, and pitch are indistinguishable in MIDI 1.0; select every duplicate and give them the same value.

The previous build's CC 119 display lane is no longer used, so Stomata does not reserve or remove ordinary CC 119 data.

## Verify

Select three notes and assign `100.0%`, `0.0%`, and `50.0%`. Loop playback. The first note should always sound, the second should never sound, and the third should sound about half the time over many repetitions. Check the MIDI Editor's **Text events** lane to confirm that the saved labels appear immediately before their notes.

## Current scope

This build requires REAPER to pass the text metadata to a JSFX during playback, as expected by the existing effect design. Verify the playback test above in the target REAPER version before using it in a production session. Old labels from the earlier `P 33.3%` format are not imported automatically; refresh and reapply the desired notes once to create the channel- and pitch-aware labels.
