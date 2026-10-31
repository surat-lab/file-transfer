# Prabhupada Corpus — Mac Codex Setup Handoff

Read this file before setting up the `research-prabhupada-corpus` skill.

## Objective

Connect the local `research-prabhupada-corpus` skill to the three Markdown corpus archives already stored in the user's OneDrive. Do not re-download, recreate, alter, or upload the corpus archives unless the user explicitly asks.

## What to restore from GitHub

Repository: `https://github.com/surat-lab/file-transfer.git`

The repository includes:

- `research-prabhupada-corpus-skill.zip` — the complete corpus skill package.
- `codex-backup-2026-10-31.zip` — the earlier workspace and skills backup.

Extract `research-prabhupada-corpus-skill.zip` so the skill is located at:

```text
~/.codex/skills/research-prabhupada-corpus/
```

## Corpus archives to locate in OneDrive

Ask the user to confirm the local macOS path for each of these files. They may appear under either the normal OneDrive folder or `~/Library/CloudStorage/`.

```text
Prabhupada_Books_NotebookLM_237_MD.zip
SP_Correspondence_NotebookLM_250_MD.zip
Srila_Prabhupada_Audio_Transcripts_NotebookLM_250_Protocol_Embedded.zip
```

Do not assume the old Windows `D:/OneDrive...` paths work on macOS.

## Required interaction with the user

Ask these questions, one at a time if necessary:

1. Is OneDrive signed in and are the three corpus ZIP files downloaded locally, rather than online-only?
2. What is the full local path to the folder that contains the three ZIP files?
3. May I update the skill configuration to use those Mac paths and build a local search index?

## Configuration work

After the user confirms the folder and grants permission:

1. Open `~/.codex/skills/research-prabhupada-corpus/assets/corpus_sources.json`.
2. Replace or add `candidate_paths` for each archive using the user-confirmed macOS paths.
3. Preserve the archive filenames, SHA-256 checksums, expected entry counts, and source metadata.
4. Do not modify the corpus ZIP archives themselves.

Example path style only — use the user's real path:

```text
/Users/<username>/Library/CloudStorage/OneDrive-HareKrishnaMovementAhmedabad/Codex Archive/.../Prabhupada_Books_NotebookLM_237_MD.zip
```

## Validate and build

From the skill directory, run:

```text
python scripts/corpus_engine.py health --config assets/corpus_sources.json
python scripts/corpus_engine.py build --config assets/corpus_sources.json
python scripts/corpus_engine.py health --config assets/corpus_sources.json
```

The durable local index should be kept at:

```text
~/.codex/corpus-indexes/prabhupada-corpus.sqlite3
```

## Completion criteria

Report success only after:

- all three archives are found locally;
- their checksums and expected entry counts validate;
- the corpus index builds successfully; and
- a small test search retrieves a source and its surrounding text.

If an archive is missing, stop and tell the user exactly which filename is missing and which OneDrive folder needs to finish syncing. Do not invent a replacement corpus or answer research queries from memory.

