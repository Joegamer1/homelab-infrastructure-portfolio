# Plex Subtitle Processing

This project standardizes embedded subtitles before media settles into the Plex library. It was added after releases with large subtitle sets created unnecessary client-selection clutter and increased the chance of avoidable playback issues.

## Goal

Keep one preferred English embedded subtitle when available, remove unnecessary embedded subtitle tracks, preserve the media file's operational metadata, and make Plex refresh its analysis afterward.

## Workflow

```text
Radarr or Sonarr import
    -> queue entry
    -> subtitle inspection
    -> conditional cleanup
    -> atomic file replacement
    -> Plex analysis request
    -> queue entry removed after success
```

The workflow supports MKV files with `mkvmerge` and MP4-family files with `ffmpeg`.

## Components

### Media post-processor

Path:

```text
/opt/scripts/media-subtitle-postprocess.sh
```

Responsibilities:

- Inspect embedded subtitle streams.
- Skip files that do not require modification.
- Keep one preferred English subtitle where available.
- Remove extra embedded subtitle streams.
- Preserve ownership, permissions, and timestamps.
- Replace the original file atomically after successful processing.

Primary log:

```text
/var/log/media-subtitle-postprocess.log
```

### Queue worker

Path:

```text
/opt/scripts/process-subtitle-queue.sh
```

Queue locations:

```text
/srv/docker/arr-stack/radarr/subtitle-queue
/srv/docker/arr-stack/sonarr/subtitle-queue
```

The worker processes imports sequentially, requests Plex analysis, and removes a queue item only after the workflow completes successfully. Failed analysis requests remain eligible for retry.

Queue log:

```text
/var/log/media-subtitle-queue.log
```

### Plex analysis helper

Path:

```text
/opt/scripts/plex-analyze-file.sh
```

The helper maps the host media path to the Plex container path, resolves the media item's Plex rating key, waits for indexing when necessary, and requests analysis for the specific item.

## Validation

The pipeline was validated across multiple independent movie imports. Tests confirmed that files containing large subtitle sets were reduced to one English embedded subtitle and that Plex analysis was requested successfully afterward.

Validation included:

- Confirming the initial subtitle count.
- Confirming the retained subtitle language.
- Verifying the final stream count with `ffprobe`.
- Confirming queue completion.
- Confirming a successful Plex analysis request.

## Existing Library Upgrade

After the automated import pipeline was proven reliable, the same cleanup process was applied as a small one-time upgrade to the existing Movies and TV libraries.

Because the library was still small, the files were processed as grouped maintenance runs rather than by building a separate recurring migration service. This brought existing media into alignment with the policy already enforced for new imports.

The maintenance run:

- Recursively selected supported video files.
- Processed files sequentially.
- Allowed compliant files to be skipped by the post-processor.
- Requested Plex analysis after successful processing.
- Continued to the next file if one item failed.

This was intentionally not scheduled. Future Radarr and Sonarr imports are already handled automatically.

## Design Decisions

- **Conditional processing:** Files are not rewritten unless cleanup is required.
- **Sequential execution:** Avoids unnecessary disk contention and makes logs easier to follow.
- **Atomic replacement:** Reduces the risk of leaving a partially written media file.
- **Metadata preservation:** Prevents cleanup from changing ownership, access, or library timing behavior.
- **Retry-safe queueing:** A Plex timing issue does not silently discard the work item.
- **Targeted Plex analysis:** Avoids depending solely on a broad library scan.

## Limitations

- The current policy retains one preferred English subtitle rather than multiple English variants.
- Forced and hearing-impaired subtitle preferences may require future policy refinement.
- External subtitle files are outside the embedded-track cleanup scope.
- Media should not be actively streamed while its container is being rewritten.

## Result

Existing and newly imported media now follow the same embedded subtitle policy. The workflow reduces subtitle clutter, keeps the process observable through logs, and removes the need for routine manual cleanup.

[Back to Plex Server](../)
