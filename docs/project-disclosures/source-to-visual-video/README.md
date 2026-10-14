# Source-to-Visual Video

## Core idea

Take a text-heavy or spoken source and replace much of the explanatory burden with visuals that express what the source is saying.

The source can be a chat log, document, project specification, README file, article, transcript, presentation material, or similar body of information.

The system identifies what should be shown, collects or generates the relevant visuals, prompts new visuals where necessary, and turns the resulting sequence into a video.

## Two closely related operations

### Collect visuals

For each meaningful part of the source, identify existing imagery, diagrams, footage, maps, screenshots, archival material, illustrations, or other visual references that communicate the content.

### Replace talk with visuals

Where spoken or written explanation can be shown more effectively, convert the underlying meaning into visual prompts and generate imagery or video that carries that part of the explanation.

The final video can therefore use narration selectively instead of treating the source as a script that must simply be read aloud.

## Basic flow

1. Ingest the source.
2. Break it into meaningful visual beats rather than arbitrary text chunks.
3. Determine what each beat is trying to communicate.
4. Decide whether that beat is best served by an existing visual, a generated image, generated video, diagram, animation, screen capture, text treatment, or another form.
5. Build prompts and retrieval queries from the source context.
6. Generate or collect candidate visuals.
7. Maintain continuity of people, places, objects, visual style, and narrative context where useful.
8. Arrange the selected material into a timed sequence.
9. Add narration, source audio, music, captions, or silence only where they strengthen the result.
10. Render the sequence as video.

## Why this is different from simply illustrating a script

The target is not one picture per paragraph.

The system should understand which parts of the source are inherently visual, which ideas require transformation before they can be visualised, which details deserve continuity across scenes, and which spoken explanation can disappear entirely once the right visual exists.

A project README, for example, could become a visual demonstration of the system it describes instead of a voice reading headings over stock footage.

## Relationship to Conversation Visuals

[Conversation Visuals](../conversation-visuals/README.md) performs contextual visual selection continuously from nearby live speech.

Source-to-Visual Video performs a deeper pass over a bounded source and can plan the complete visual sequence before rendering.

## Status

Concept captured for further development and exposure.
