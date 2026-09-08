# Step 08: Music, post and delivery

> **Executor**: Main Agent + audio tools (TTS, ffmpeg)
> **Input**: `assembled.mp4`, `04-script.md`, `config/default.yaml`
> **Output**: final package — video, subtitles, audio, manifest
> **Runs only in full mode.** In prompts-only mode this step is not reached.

## Execution Instructions

1. Confirm `assembled.mp4` exists. Otherwise stop.
2. **Narration**: generate TTS from the five script lines. Normalize each line to `audio.narration_lufs` (-14 LUFS). Do not reuse narration from an earlier run.
3. **Music**: add a quiet background track at `audio.music_volume` (8%) — it must never compete with the narration.
4. **Mix**: merge narration + music. No global loudness normalization — tracks are pre-calibrated.
5. **Subtitles**: transcribe the final audio for real timestamps, correct the words against the script, and burn the subtitles in (thin outline, lower third). Save a separate copy without subtitles.
6. **Deliver**: export the final files (with and without subtitles) and write a delivery manifest listing every file, the duration, the loudness, and any caveats.
7. Follow `references/rules/voiceover-rules.md` for line timing. Mark the "Needs your call" items from the truth sheet as still open in the manifest when they were not decided.

## Input Files

| File | Source | Description |
|------|--------|-------------|
| `{run_folder}/assembled.mp4` | Step 07 output | Stitched video |
| `{run_folder}/04-script.md` | Step 04 output | The five timed voiceover lines |

## Output Files

| File | Format | Description |
|------|--------|-------------|
| `final.mp4` | MP4 | Video without subtitles |
| `final-subtitled.mp4` | MP4 | Video with burned-in subtitles |
| `subtitles.srt` | SRT | Timed subtitle file |
| `narration.wav` | WAV | TTS narration track |
| `manifest.md` | Markdown | Delivery manifest |

## Validation Checkpoints

| ID | Check | Pass Standard |
|----|-------|---------------|
| 08-a | All files present | final, final-subtitled, srt, narration, manifest exist |
| 08-b | Loudness | Narration at -14 LUFS |
| 08-c | Subtitles match script | Transcription corrected against the five lines |
| 08-d | Manifest complete | Lists files, duration, loudness, open items |

## Next Step

→ Delivery. The run is complete.
