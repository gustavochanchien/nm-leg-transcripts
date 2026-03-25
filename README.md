# New Mexico Legislature Transcripts

Machine-generated transcripts of New Mexico House proceedings from the 2026 Regular Session (January 20 -- February 19, 2026).

## How these transcripts are produced

These transcripts are created through an automated pipeline:

1. **Recording discovery** -- The pipeline queries the NM Legislature's [Harmony webcast system](https://sg001-harmony.sliq.net/00293/harmony) to find available recordings of floor sessions and committee meetings.

2. **Audio extraction** -- A headless browser (Playwright) navigates each Harmony page to resolve the actual video stream URL, then FFmpeg downloads and converts the audio to MP3 (64kbps, 16kHz mono -- optimized for speech-to-text).

3. **Transcription** -- [faster-whisper](https://github.com/SYSTRAN/faster-whisper) (an optimized implementation of OpenAI's Whisper model) transcribes the audio into timestamped text segments.

4. **Speaker identification and structuring** -- The raw transcript is sent to Google Gemini 3 Flash, which identifies speakers from contextual clues (e.g. "The chair recognizes Representative Garcia"), combines fragmented sentences into coherent paragraphs, and extracts metadata including bills discussed, votes taken, and committee actions.

The full pipeline source code is at [github.com/scottyg/legissitant](https://github.com/scottyg/legissitant) (if public) or in the parent directory of this repository.

## Update frequency

The pipeline runs daily during the legislative session. New recordings typically appear on Harmony within a few hours of a proceeding and are processed on the next pipeline run. Each update is committed to this repository with a timestamp.

To see when this repository was last updated, check the most recent commit or the `last_updated` field in `index.json`.

## Accuracy

These are machine-generated transcripts. Expect occasional errors in:

- **Speech-to-text** -- Whisper may mishear proper nouns, bill numbers, or quiet speakers. Accuracy improves with larger Whisper models but is never perfect.
- **Speaker identification** -- Gemini infers who is speaking from context. Each speaker in the `speaker_map` includes a confidence score. Speakers labeled "Unknown" could not be reliably identified.
- **Metadata extraction** -- Bill numbers, vote counts, and committee actions are extracted by AI and may occasionally be incorrect.

These transcripts are not official records. For the official record, refer to the [NM Legislature website](https://www.nmlegis.gov/).

## File format

Each JSON transcript file contains:

| Field | Description |
|---|---|
| `session` | Session identifier (e.g. `2026-regular`) |
| `date` | Date of the proceeding |
| `committee` | Committee or chamber name |
| `start_time` / `end_time` | Time range of the recording |
| `location` | Room or chamber |
| `bills_discussed` | Array of bills with title and sponsor |
| `votes` | Array of votes with result and counts |
| `speaker_map` | Identified speakers with confidence scores |
| `turns` | Array of speaker turns with timestamp and text |
| `full_text` | Complete readable transcript as a single string |
| `summary` | AI-generated executive summary |

`index.json` contains the metadata for all transcripts (everything except `turns` and `full_text`) and a `last_updated` timestamp, used by the website.

## Website

Open `index.html` in a browser or serve this directory with any static file server. The site supports searching across all transcripts by bill number, speaker name, committee, or any keyword.

## License

The underlying legislative proceedings are public record. The transcription and structuring are provided as-is with no warranty of accuracy.
